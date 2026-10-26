# SOC Lab Write-Ups

Hands-on lab write-ups from my path toward a SOC analyst role. Each lab simulates an attacker technique in an isolated home lab, then investigates what the SIEM detected, what it missed, and which alerts were false positives.

## Lab Environment

- **Hypervisor:** Oracle VirtualBox
- **Windows endpoint:** Windows 11 Enterprise Evaluation with Sysmon, Wireshark, Atomic Red Team, and the Wazuh agent
- **SIEM server:** Ubuntu Server 26.04.1 LTS running Wazuh 4.14.9 (indexer, server, and dashboard)
- **Network:** both VMs on an isolated VirtualBox NAT Network, separate from my home network

All attack activity is simulated with Atomic Red Team inside the lab VMs. No real malware is used.

## Labs

| # | Lab | MITRE ATT&CK | Write-up |
|---|---|---|---|
| 1 | System Owner/User Discovery | T1033 | [Day1-T1033-Wazuh-Writeup.md](Day1-T1033-Wazuh-Writeup.md) |

### Lab 1: Key Findings

- Wazuh detected the discovery commands (`whoami /all`, `net user`) as generic discovery activity (rule 92031, level 3), but tagged them as Account Discovery, not T1033.
- The Atomic Red Team framework itself triggered PowerShell and `cmd.exe` alerts (rules 92027 and 92052).
- Two alerts were unexpected and needed triage: a level 12 `dllhost.exe` alert (rule 61638) that looked like normal Windows behavior, and five level 15 "executable dropped" alerts (rule 92213) that pointed to a PowerShell script-policy test file.
- Takeaway: alert severity does not equal malicious activity. Parent process, command line, file path, and signature decide it.

## Skills Demonstrated

- Building and configuring a SIEM lab (Wazuh, Sysmon, VirtualBox)
- Simulating attacker techniques with Atomic Red Team and mapping them to MITRE ATT&CK
- Triaging alerts and separating true detections from false positives
- Documenting detection gaps and proposing improvements

## Next Steps

- Write a custom Wazuh rule that tags `whoami` and `net user` activity as T1033
- Re-run the test from a clean snapshot and compare the results
- Add more techniques and write-ups

## About Me

[Electronics Engineering graduate with CompTIA Security+ and ISC2 CC certifications, building hands-on SOC skills in a Wazuh and Sysmon home lab. Seeking a SOC analyst or junior cybersecurity role, and open to IT support roles.]

CompTIA Security+ | ISC2 CC | [LinkedIn URL]
