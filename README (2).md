# Logjammer: suspicious user activity on Windows (Event Logs)

**Platform:** Hack The Box Sherlock
**Data sources:** Windows Event Logs (.evtx), opened in Event Viewer
**Tools:** Event Viewer, filter by Event ID

## Summary
[2–3 sentences: a user logged in, changed the audit policy and created a firewall rule that allowed outbound traffic on port 4444.]

## Timeline (UTC)
Event Viewer shows local time. I converted to UTC before writing the timeline.

| Time | Event | Event ID |
|---|---|---|
| [time] | First logon of the user | 4624 |
| [time] | Outbound firewall rule created | [firewall event] |
| [time] | Audit policy changed | 4719 |

## Evidence
Which events I filtered and what fields I read (Logon Type, Account Name, SID, rule name, direction, port).

## IOCs
| Type | Value | Context |
|---|---|---|
| Firewall rule | [rule name] | Outbound, TCP 4444 (commonly used by Metasploit) |
| Account | [user] | |

## MITRE ATT&CK
| Tactic | Technique | Evidence |
|---|---|---|
| Defense Evasion | T1562 Impair Defenses | Firewall rule and audit policy changed |
| Command and Control | T1571 Non-Standard Port | Outbound TCP 4444 |

## Verdict and next steps
- Verdict: true positive
- Escalation note for L2: [4 lines]
