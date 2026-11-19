# SOC lab write-ups

Incident reports from hands-on blue team labs. These are training exercises, not work experience.
Every report follows the same format: **Summary → Timeline → Evidence → IOCs → MITRE ATT&CK → Verdict and next steps**.
Flags and answers are not published. The reports describe the method and the evidence.

| Lab | Platform | Data | Main skills |
|---|---|---|---|
| [Brutus](brutus) | Hack The Box Sherlock | auth.log, wtmp | Brute force, session tracking, persistence |
| [Hammered](hammered) | CyberDefenders | auth.log | Failed-login analysis, attacking IPs |
| [Logjammer](logjammer) | Hack The Box Sherlock | Windows .evtx | 4624, firewall rules, audit policy changes |
| [Unit42](unit42) | Hack The Box Sherlock | Sysmon .evtx (Splunk) | Process creation, DNS, file creation, process tree |
| [Campfire-1](campfire-1) | Hack The Box Sherlock | Windows Security .evtx, Prefetch (Splunk) | Kerberoasting (4769), process creation, execution evidence |
## Report template

See [TEMPLATE.md](TEMPLATE.md).
