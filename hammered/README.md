# Hammered: SSH brute force against a Linux server (auth.log)

**Platform:** CyberDefenders
**Data sources:** auth.log
**Tools:** grep, awk, sort, uniq

## Summary
[2–3 sentences: the server received thousands of failed SSH logins from many IPs. Describe whether any login succeeded.]

## Timeline (UTC)
| Time | Event | Source |
|---|---|---|
| [time] | First failed logins | auth.log |
| [time] | [Successful login, if any] | auth.log |

## Evidence
Ranking the sources of failed logins:

```bash
grep 'Failed password' auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head
```

Top source: 219.150.161.20 with 9,259 failed attempts. In total 46 unique IPs failed to log in.
Note: lines for invalid users have an extra column, so the IP is not always in the same place. I checked this separately.

## IOCs
| Type | Value | Context |
|---|---|---|
| IP | 219.150.161.20 | Most failed logins (9,259) |
| IP | 8.12.45.242 | Second source (3,037) |
| IP | [others] | |

## MITRE ATT&CK
| Tactic | Technique | Evidence |
|---|---|---|
| Credential Access | T1110 Brute Force | Thousands of failed password attempts |

## Verdict and next steps
- Verdict: true positive
- Recommendation: block the source IPs, disable password login for SSH, use keys and rate limiting.
