# Hammered: SSH brute force and root compromise (auth.log)

- **Platform:** CyberDefenders
- **Data sources:** auth.log (102,164 events)
- **Tools:** grep, awk, sort, uniq, Splunk (SPL)

## Summary
An internet-facing Linux server (app-1) was brute-forced over SSH from 46 unique IP addresses.
Six of them eventually logged in as root with a password. The first successful login came from
219.150.161.20 after 9,259 failed attempts. Root login over SSH with password authentication
was enabled, which made the compromise possible.

## Timeline (log times, year not recorded in the log)
| Time | Event | Source |
|---|---|---|
| Apr 19 05:41:44 | First root login from 219.150.161.20 | auth.log |
| Apr 19 05:42:27, 05:55:20, 05:56:05 | Three more root logins from the same IP | auth.log |
| Apr 19 10:45:36 | Root login from 222.66.204.246 | auth.log |
| Apr 20 06:13:03 | Root login from 121.11.66.70 | auth.log |
| Apr 22 11:02:15 | Root login from 222.169.224.197 | auth.log |
| Apr 23 03:11:03, 03:20:41 | Root logins from 122.226.202.12 | auth.log |
| Apr 24 11:36:19 | Root login from 121.11.66.70 (again) | auth.log |
| Apr 24 15:28:37 | Root login from 61.168.227.12 | auth.log |

## Evidence
Failed logins per source IP (terminal, then the same result in Splunk):

grep 'Failed password' auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head

Source IPs with both failed and accepted logins (Splunk):

source="auth.log" ("Failed password" OR "Accepted password")
| rex "(?<result>Failed|Accepted) password for (invalid user )?(?<user>\S+) from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count(eval(result="Failed")) as failed count(eval(result="Accepted")) as accepted by src_ip
| where accepted > 0 | sort -failed

Many failed logins followed by a successful one is the pattern of a successful brute force.
Sources with few failures and many successes (for example internal 10.0.1.x addresses) look
like normal users.

## IOCs
| Type | Value | Context |
|---|---|---|
| IP | 219.150.161.20 | 9,259 failed logins, 4 root logins |
| IP | 222.66.204.246 | 1,573 failed, 1 root login |
| IP | 121.11.66.70 | 1,435 failed, 2 root logins |
| IP | 222.169.224.197 | 646 failed, 1 root login |
| IP | 122.226.202.12 | 513 failed, 2 root logins |
| IP | 61.168.227.12 | 213 failed, 1 root login |
| Account | root | Logged in over SSH with a password |

## MITRE ATT&CK
| Tactic | Technique | Evidence |
|---|---|---|
| Credential Access | T1110.001 Brute Force: Password Guessing | Thousands of failed SSH logins |
| Initial Access | T1078.003 Valid Accounts: Local Accounts | Successful root logins |

## Limits of this evidence
auth.log does not record commands typed in a session, so what the attacker did after
logging in is unknown. No useradd, usermod, passwd or sudo events appear after the first login.

## Verdict and next steps
- Verdict: true positive, the root account is compromised.
- Containment: isolate the host, block the six IPs, reset the root password.
- Hardening: disable root SSH login (PermitRootLogin no), use key-based authentication, add rate limiting (fail2ban).
- Escalation note for L2: please review .bash_history, crontabs and authorized_keys, and check for other hosts with the same exposure.
