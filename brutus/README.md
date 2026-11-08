# Brutus: SSH brute force and persistence on a Linux server

**Platform:** Hack The Box Sherlock
**Data sources:** auth.log, wtmp
**Tools:** Python wtmp parser (`utmp.py`), grep, awk

## Summary
[2–3 sentences: an attacker brute-forced SSH, logged in, created a new account for persistence and downloaded a script.]

## Timeline (UTC)
| Time | Event | Source |
|---|---|---|
| [time] | Brute force from [IP] begins | auth.log |
| [time] | Successful login | auth.log |
| [time] | Session 37 opens | wtmp / auth.log |
| [time] | New account created (`useradd`) | auth.log |
| [time] | Script downloaded with `curl` | [source] |
| [time] | Session closes | auth.log |

## Evidence
How I decoded wtmp and matched the attacker's session to auth.log.

```bash
python3 utmp.py wtmp -o wtmp_output.tsv
grep "Accepted password" auth.log
```

## IOCs
| Type | Value | Context |
|---|---|---|
| IP | [attacker IP] | Source of brute force and login |
| Account | [new account name] | Created for persistence |
| File | linper.sh | Persistence script downloaded with curl |

## MITRE ATT&CK
| Tactic | Technique | Evidence |
|---|---|---|
| Credential Access | T1110 Brute Force | Many failed SSH logins before success |
| Persistence | T1136.001 Create Account: Local Account | New account created after login |
| Command and Control | T1105 Ingress Tool Transfer | Script downloaded with curl |

## Verdict and next steps
- Verdict: true positive
- Containment: [what you would do]
- Escalation note for L2: [4 lines: Observation / Assessment / Actions taken / Request to L2]
