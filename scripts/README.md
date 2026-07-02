# The Automation Engine

This directory contains the custom scripts, payloads, and automation frameworks utilized by Rekt Academy operators. Manual hacking doesn't scale. If you are running the same commands twice, you should be writing a script. 

These tools are designed to streamline reconnaissance, automate exploitation, and break infrastructure faster. 

## ⚙️ Core Toolsets

### `/FUZZERS`
Custom fuzzing frameworks designed to brute-force edge cases in both traditional applications and smart contracts. Expect highly parallelized scripts for discovering input validation failures, memory corruption vectors, and unexpected state transitions in Web3 protocols.

### `/MALWARE GENERATORS`
Templates and automated builders for generating weaponized payloads. From polymorphic shellcode wrappers to specialized stagers designed to bypass modern EDR (Endpoint Detection and Response) solutions. 

### `/NMAP SCRIPTS`
Proprietary Nmap Scripting Engine (NSE) scripts used for advanced network enumeration. These scripts go beyond standard port scanning, looking for specific CVEs, misconfigurations, and obscure service banners across our Web2 lab environments.

### `/OFFENSIVE PYTHON`
The duct tape of the hacking world. A collection of standalone Python scripts for exploiting specific vulnerabilities, cracking hashes, automating API abuse, and crafting custom network packets (Scapy). 

### `/VPN DEBUG`
Diagnostic tools for troubleshooting the `lab-net`. If your OpenVPN connection drops, or you can't route traffic into the target environment, use these scripts to unfuck your routing tables and DNS configurations before complaining in the Telegram chat.

---

### ⚠️ /etc/rules.txt

1. **Review Before Execution.** Never blindly run a script you didn't write. Always review the source code.
2. **Target Isolation.** These scripts are highly aggressive. Only point them at authorized targets or local lab environments.

Automate everything.
