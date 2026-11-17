## Timeline (UTC, from Sysmon `UtcTime`)

| Time (UTC) | Event | Sysmon ID |
|---|---|---|
| 03:41:26] | Browser resolves a Dropbox domain (malware delivery) | 22 |
| 03:41:26.459 | `firefox.exe` writes `Preventivo24.02.14.exe.exe` to `C:\Users\CyberJunkie\Downloads\` | 11 |
| 03:41:56.538 | User double-clicks the file; process starts, parent `explorer.exe` | 1 |
| 03:41:56.955 | The process queries `www.example.com` (connectivity check) | 22 |
| 03:41:57.905 | `msiexec.exe /i main1.msi` started, parent is the malicious process, MSI path under `AppData\Roaming\Photo and Fax Vn\` | 1 |
| 03:41:58.389 to 03:41:58.795 | Several files created under C:\\Users\\CyberJunkie\\AppData\\Roaming\\Photo and Fax Vn\\Photo and vn 1.1.2\\install\F97891C\`, including `once.cmd` | 11 |
| 03:41:58.795 | The malicious process terminates (about 2.3 s after start) | 5 |
