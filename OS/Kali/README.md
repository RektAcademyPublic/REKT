# Kali Linux Download Sources

This repository provides direct links to the official Kali Linux images recommended for use with Rekt Academy's interactive lab environments. 

> [!WARNING]
> Always verify the SHA256 checksums of any downloaded ISO or VM image before using it in a lab environment.

## 1. Virtual Machines (Recommended for Labs)
Pre-built VM images are the fastest way to get started with Rekt Academy's vulnerability labs. These images come with the guest additions pre-installed.

* **VMware (64-bit):** [Download VMware Image](https://www.kali.org/get-kali/#kali-virtual-machines)
* **VirtualBox (64-bit):** [Download VirtualBox Image](https://www.kali.org/get-kali/#kali-virtual-machines)
* **Hyper-V (64-bit):** [Download Hyper-V Image](https://www.kali.org/get-kali/#kali-virtual-machines)

## 2. Bare Metal (Installer Images)
For installing Kali Linux natively on a machine or dual-booting.

* **Installer (64-bit):** [Download ISO](https://www.kali.org/get-kali/#kali-installer-images)
* **Live Boot (64-bit):** [Download Live ISO](https://www.kali.org/get-kali/#kali-live) - Useful for running Kali from a USB stick without installing.

## 3. Windows Subsystem for Linux (WSL)
If you are developing on a Windows machine, you can run Kali directly via WSL for command-line access to all tools.

* **Microsoft Store:** [Get Kali Linux for WSL](https://apps.microsoft.com/store/detail/kali-linux/9PKR34TNCV07)
* *Note: Run `sudo apt update && sudo apt install kali-win-kex` after installing to get a graphical desktop experience.*

## 4. Docker Images
For quickly spinning up isolated pentesting tools without the overhead of a full VM.

* **Docker Hub:** [kali-rolling](https://hub.docker.com/u/kalilinux)
* **Usage:** `docker run -t -i kalilinux/kali-rolling /bin/bash`

---
*For older releases or specific architecture builds (like ARM64 for Apple Silicon), please visit the official [Kali Linux Archive](https://old.kali.org/).*
