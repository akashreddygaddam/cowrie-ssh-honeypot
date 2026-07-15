\# SSH Honeypot with Cowrie — Threat Detection Lab



\## Overview

A deployed and monitored SSH honeypot using Cowrie to capture and analyze

unauthorized login attempts and attacker behavior, simulating real-world

SOC log analysis workflows.



\## Architecture

Attacker (host machine) --SSH--> Ubuntu 24.04 VM running Cowrie (isolated,

NAT network) --> JSON + text logs



\## What I Did

\- Deployed Cowrie (v3.0.6) on a dedicated, non-root Linux user for security best practice

\- Configured iptables to redirect port 22 -> Cowrie's listener on port 2222

\- Generated and captured multiple simulated attack sessions from a separate host machine

\- Analyzed structured JSON logs for credentials attempted and commands run

\- Replayed attacker TTY sessions using Cowrie's playlog tool

\- Documented findings on attacker behavior patterns



\## Skills Demonstrated

Linux system administration, network configuration (VMware NAT networking),

iptables/NAT port redirection, log analysis, understanding of attacker TTPs

(Tactics, Techniques, Procedures), troubleshooting real-world deployment issues.



\## Key Findings

See \[analysis/findings.md](analysis/findings.md) for the full write-up.



I ran 3 separate simulated SSH sessions (root, admin, test credentials), all

accepted by the honeypot. Commands run included system fingerprinting

(`whoami`, `uname -a`), reconnaissance (`cat /etc/passwd`, `ls -la`), and a

simulated payload download (`wget`), mirroring real-world attacker behavior.

The structured JSON log captured session IDs, source IPs/ports, exact

timestamps, and file hashes — the same format a SIEM tool would ingest for

correlation and alerting.



\## Screenshots

!\[Login attempt](screenshots/login-attempt.png)

!\[Session replay](screenshots/playlog-replay.png)

!\[JSON log sample](screenshots/json-log-sample.png)



\## Tools Used

Cowrie, VMware Workstation, Ubuntu Server 24.04, iptables, Python/pip

