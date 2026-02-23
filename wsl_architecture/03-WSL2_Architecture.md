
# WSL 2 Architecture

WSL2 uses a real Microsoft-packaged Linux kernel updated via Windows Update.

## Lightweight VM Initialization
* Windows launches a tiny Hyper-V utility VM.
* The Linux kernel boots instantly.
* Your distro's filesystem is mounted from a **VHDX-backed ext4 filesystem**.

## System Calls Handling
All system calls run natively inside the Linux kernel — no translation layer. This makes WSL 2 highly compatible with Docker and other kernel-heavy workloads.

## GPU / ML Support
WSL2 supports GPU compute (CUDA, DirectML) via paravirtualized GPU passthrough.

**Advantages:**
* Better performance for git clone, npm install, package managers.
* Fast IO with ext4.vhdx.

**Disadvantages:**
* Slower access to Windows files from WSL2 compared to WSL1. Since WSL1 uses NT translation.
