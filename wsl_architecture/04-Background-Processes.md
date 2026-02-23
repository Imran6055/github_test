
# Background Processes in WSL

When WSL runs, several Windows-side processes start.

![WSL Processes](../../../../.attachments/WSL_Background_Process_in_Windows.png)

##  wslservice.exe - The Core Orchestrator
`wslservice.exe` is the main Windows service responsible for:

* Receiving requests to start or manage WSL distros
* Handling CreateInstance() calls
* Deciding whether to launch a WSL1 instance or boot the WSL2 Virtual Machine
* Creating and configuring the VM using HCS (Host Compute System) for WSL2

WSL2 startup flow involves serializing VM config into a JSON and passing it to HcsCreateComputeSystem() to spawn the lightweight Linux VM.

## LxssManager — The Subsystem Manager Service
LxssManager (Linux Subsystem Manager) is a critical Windows service that:

* Starts and stops WSL instances
* Tracks which Linux distributions are registered
* Monitors all WSL processes for a user
* Launches Linux processes through LX Instance structures
* Prevents access during install/uninstall events

If **LxssManager** hangs, WSL commands such as `wsl, wsl --list`, or distro startup freeze.

## wslhost.exe — Host for Linux Processes (WSL2)
`wslhost.exe` appears in Task Manager when WSL2 is running.

* Bridges Windows <→> Linux communication
* Connects IO streams via Hyper-V Sockets (hvSockets)
* Handles filesystem redirection (9P server for `\wsl$`).
WSL2 uses a small **Hyper‑V VM**, and **wslhost.exe** acts as the interface between VM processes and Windows.

## LXCoreManager.dll (Also appears as lxcore.sys/lxss.sys) (WSL1)

This component is part of WSL1 internals but participates indirectly in WSL2:

* Implements Linux syscall translation for WSL1
* Receives forwarded syscalls via NT kernel
* Maintains compatibility logic for ELF (Executable and Linkable Format) binaries
* Registers LX processes through LX Instances

WSL1 drivers include lxcore.sys and lxss.sys, acting as pico providers.

## Difference Between WSL1 and WSL2 Background Process

![WSL1 vs WSL2 Processes](../../../../.attachments/WSL1_vs_WSL2_Background_Process.png)
