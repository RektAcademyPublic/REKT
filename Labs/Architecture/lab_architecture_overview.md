# Rekt Academy Lab Infrastructure Architecture

This document provides a technical overview of the Rekt Academy lab infrastructure, detailing the network topology, VPN configuration, security isolation, and routing rules that power the learning environment.

## 1. Network Topology & Isolation

The core design principle for the lab infrastructure is **Internal-Only Isolation**. Vulnerable environments are strictly prohibited from exposing ports to the public internet.

- **The Bridge Network**: Labs are deployed onto a dedicated containerized bridge network named `lab-net` using the `10.20.0.0/16` subnet.
- **Internal Binding**: Lab containers (e.g., Damn Vulnerable DeFi, Metasploitable) listen on internal ports (`0.0.0.0` within the container context) but **do not** map these ports to the host (`PortBindings` are omitted). 
- **Gateway**: A dedicated `udp-vpn` container is attached to both the host network and the `lab-net` bridge, acting as the sole gateway for inbound student traffic.

## 2. Client Access: OpenVPN & Kali/ZIION OS

To ensure compatibility with modern penetration testing distributions like **Kali Linux** and **ZIION OS**, the default OpenVPN configurations are rigorously hardened.

- **The "Surgery" Process**: Legacy options that break modern Linux network stacks or DNS resolution (such as `comp-lzo` and `block-outside-dns`) are automatically stripped from the OpenVPN packs during generation.
- **Hardened Cryptography**: The VPN enforces `AES-256-GCM` as the primary cipher, falling back to `AES-256-CBC` only if necessary.
- **Topology**: We use the `subnet` topology rather than legacy p2p addressing.
- **Transport**: The VPN operates over TCP port 1194, providing higher reliability for students connecting from restricted corporate or academic networks.

## 3. Subnets, Routing, and Firewall Rules

When a student connects, they are assigned an IP address from the VPN client pool (`192.168.255.0/24`). The server then pushes the lab route directly to the client:
`push "route 10.20.0.0 255.255.0.0"`

To ensure traffic flows correctly from the VPN tunnel to the isolated `lab-net`, the host implements specific firewall and kernel routing rules:

### IP Forwarding
The host kernel is configured to allow packet forwarding between interfaces:
```bash
sysctl -w net.ipv4.ip_forward=1
```

### IPTables NAT & Masquerading
Traffic originating from the VPN client subnet is masqueraded (NAT'd) as it enters the bridge network, ensuring lab containers can respond correctly:
```bash
iptables -t nat -A POSTROUTING -s 192.168.255.0/24 -o eth+ -j MASQUERADE
```

### FORWARD Rules
Explicit `ACCEPT` rules are placed in the `FORWARD` chain to permit traffic specifically between the VPN tunnel (`tun0`) and the bridge interface (`eth1`):
```bash
iptables -A FORWARD -i tun0 -o eth1 -j ACCEPT
iptables -A FORWARD -i eth1 -o tun0 -j ACCEPT
```

## 4. Resilience: Zero-Dependency Fallback

To maintain 100% platform uptime for entry-level modules (e.g., Linux Fundamentals) even during Docker subsystem failures or resource-constrained periods, the architecture includes a fallback **MockShell** simulation.

- **MockShell.js**: A Node.js-based terminal emulator providing an in-memory Virtual File System (VFS).
- **Safe Environment**: It natively simulates commands like `cat`, `grep`, `cd`, and `chmod`, allowing users to practice CLI navigation safely without requiring the overhead of a dedicated container.

## 5. Web3 & Blockchain Lab Architecture

For advanced Web3 and smart contract hacking tracks, the infrastructure accommodates specialized blockchain environments, tools, and dedicated RPC endpoints.

### Web3 Hacklab Boxes (e.g., Damn Vulnerable DeFi)
The DeFi hacklab boxes use a custom multi-stage Docker pattern designed for offensive security testing against smart contracts. 
- **Foundry & Node Tooling**: Images are built with a complete Web3 toolchain including **Foundry** (forge, cast, anvil) and Node.js for Hardhat compatibility.
- **Dedicated Static IPs**: Each lab is assigned a static internal IP (e.g., `10.0.0.7`) to allow students consistent connection via the VPN.
- **Service Exposure**:
  - **RPC Endpoints**: The local EVM node (Anvil/Hardhat) exposes port `8545` directly on the container's `0.0.0.0` address, routing through the bridge.
  - **SSH Access**: Containers are configured with an OpenSSH server (`port 22`) for interactive debugging and script execution inside the environment.
  - **Web UIs**: Custom entrypoint scripts (e.g., running `python3 -m http.server`) serve web-based CTF frontends on `port 80`.
- **Security Check**: As with Web2 labs, these ports are only bound internally to `lab-net`, mitigating the risk of public internet scanning.

### Archive Node Hosting
To provide students with realistic, mainnet-forked environments without excessive public RPC rate-limiting, the architecture supports hosting dedicated archive infrastructure:
- **EVM Archive Nodes (Geth/Erigon)**: For full historical state access, heavy execution clients can be deployed onto high-IOPS storage instances and attached directly to the `lab-net` subnet. 
- **Internal RPC Routing**: Lab containers (like the DeFi hacklabs) can be configured to fork their local testing environments from this internal archive node rather than an external provider (like Infura/Alchemy), drastically reducing latency and external API costs.
- **On-Demand Forking**: Tools like Anvil are spun up with `anvil --fork-url http://<internal-archive-ip>:8545` inside the student's lab container, allowing each student to manipulate a private fork of the blockchain state while relying on the shared archive node for historical data.
