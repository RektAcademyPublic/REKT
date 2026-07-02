# The Proving Grounds

This directory contains the core simulation environments and vulnerable configurations utilized in the Rekt Academy curriculum. We do not believe in theoretical security. To understand an exploit, you must execute it against a live, hostile environment. 

These labs are designed to break. Your job is to break them first.

## 🔬 Infrastructure Overview

### `/Web2`
Legacy infrastructure and traditional enterprise vulnerabilities. This directory contains the configuration files, target architectures, and VPN connectivity packs required to access our cloud-hosted Web2 labs. Expect Active Directory exploitation, classic web application vulnerabilities, and network-level pivoting. 

### `/Web3`
The Wild West of decentralized finance. This directory tracks vulnerable smart contracts, decentralized application (dApp) logic flaws, and protocol-level exploits. We have ported industry-leading vulnerable repositories (including the DarkRelay labs) directly into our ecosystem so you can practice reentrancy, oracle manipulation, and flash loan attacks in a safe, reproducible environment.

### `/Hybrid`
The convergence. The line between Web2 and Web3 is disappearing. This directory contains advanced scenarios where you must pivot from traditional Web2 infrastructure (e.g., compromising a centralized key-management server or Web2 frontend) to execute on-chain Web3 exploits. This is where real-world multi-stage attacks are simulated.

---

### ⚠️ /etc/rules.txt

1. **Zero Local Configuration Required.** We host all active targets dynamically in our cloud infrastructure. Do not attempt to run these environments locally unless specifically instructed. Use the provided OpenVPN configurations to connect to `lab-net`.
2. **Containment Protocol.** Treat all downloaded contracts and binaries as hostile. 

Welcome to the firing range.
