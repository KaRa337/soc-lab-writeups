# Campfire-1: Kerberoasting investigation (Windows Security logs)

**Platform:** Hack The Box Sherlock
**Status:** In progress (draft, details being completed)
**Data sources:** Windows Security .evtx, Prefetch files
**Tools:** Splunk (SPL), PECmd, Timeline Explorer

## Summary
[2-3 sentences: what attack it was, which account, what the attacker ran.]

## Investigation approach
| Step | Query / tool | Why |
|---|---|---|
| Overview | `index=<ww> \| stats count by EventCode` | See which event types exist |
| Kerberos tickets | `index=<ww> EventCode=4769` | Service ticket requests (Kerberoasting target) |
| Weak encryption | `index=<ww> "0x17" EventCode=4769` | RC4 tickets, typical for Kerberoasting |
| Process creation | `index=<ww> EventCode=4688` | Which programs ran and with what command line |
| PowerShell | `index=<ww> EventCode=4104` | Script block contents |
| Group enumeration | `index=<ww> EventCode=4799` | [what you found] |
| Execution evidence | PECmd, then Timeline Explorer | Prefetch shows the attacker tool was executed |

## Timeline (UTC)
| Time | Event | Event ID / source |
|---|---|---|
| [time] | [what happened] | [ID] |

## IOCs
| Type | Value | Context |
|---|---|---|
| Tool | [name] | Found in Prefetch |
| Account | [user] | Requested the tickets |
| Client IP | [IP] | Source of the ticket requests |
| Targeted service | [name] | Service account whose ticket was requested |

## MITRE ATT&CK
| Tactic | Technique | Evidence |
|---|---|---|
| Credential Access | T1558.003 Kerberoasting | Many 4769 events with RC4 (0x17) |

## Verdict and next steps
- Verdict: true positive
- Recommendation: isolate host, reset the user's password and the service accounts' passwords, review activity after the compromise.
