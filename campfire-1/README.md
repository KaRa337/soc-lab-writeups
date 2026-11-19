# Campfire-1: Kerberoasting investigation (Windows Security logs)

**Platform:** Hack The Box Sherlock
**Status:** In progress (draft, details being completed)
**Data sources:** Windows Security .evtx, Prefetch files
**Tools:** Splunk (SPL), PECmd, Timeline Explorer

## Summary
On 2024-05-21, between 03:05 and 03:20 UTC, the account alonzo.spire@FORELA.LOCAL requested Kerberos service tickets with RC4 encryption (event 4769), including for the MSSQLService service account, from the internal host 172.17.79.129. Prefetch shows that Rubeus.exe was executed from the user's Downloads folder. This pattern is consistent with Kerberoasting (T1558.003): the tickets can be cracked offline, which is not visible in the logs.

## Investigation approach
| Step | Query / tool | Why |
|---|---|---|
| Overview | `index=<ww> \| stats count by EventCode` | See which event types exist |
| Kerberos tickets | `index=<ww> EventCode=4769` | Service ticket requests (Kerberoasting target) |
| Weak encryption | `index=<ww> "0x17" EventCode=4769` | RC4 tickets, typical for Kerberoasting |
| Process creation | `index=<ww> EventCode=4688` | Which programs ran and with what command line |
| PowerShell | `index=<ww> EventCode=4104` | Script block contents |
| Execution evidence | PECmd, then Timeline Explorer | Prefetch shows the attacker tool was executed |

## Timeline (UTC)
| Time | Event | Event ID / source |
|---|---|---|
| 2024-05-21 03:05:54 | First 4769 service ticket request with RC4 (0x17) | 4769 |
| 2024-05-21 03:16:32 | Rubeus.exe executed from the user's Downloads folder | Prefetch |
| 2024-05-21 03:20:24 | Last 4769 request with RC4 (0x17) | 4769 |

## IOCs
| Type | Value | Context |
|---|---|---|
| Tool | Rubeus.exe | Kerberos abuse tool, found in Prefetch |
| Path | C:\Users\Alonzo.spire\Downloads\Rubeus.exe | Where the tool was run from |
| Account | alonzo.spire@FORELA.LOCAL | Requested the service tickets |
| Client IP | 172.17.79.129 | Internal source of the ticket requests |
| Targeted service | MSSQLService | Service account whose ticket was requested |

## MITRE ATT&CK
| Tactic | Technique | Evidence |
|---|---|---|
| Credential Access | T1558.003 Kerberoasting | Many 4769 events with RC4 (0x17) |

## Verdict and next steps
- Verdict: true positive
- Recommendation: isolate host, reset the user's password and the service accounts' passwords, review activity after the compromise.
