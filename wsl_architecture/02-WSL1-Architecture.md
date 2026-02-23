
# WSL 1 Architecture

## Process Initialization: Pico Processes

When you run a Linux binary:

1. Windows creates a **pico process**, a minimal process type introduced specifically for WSL.
2. No Windows DLLs are loaded; instead, the Linux ELF binary is mapped directly.
3. A **pico provider** driver handles Linux-specific operations.

This allows ELF binaries to run natively on the Windows kernel.

## System Call Handling
Linux applications issue syscalls → Windows detects the process is a **pico process** → calls are redirected to the WSL kernel emulation layer → the pico provider translates each Linux syscall into its NT kernel equivalent.

## fork() Emulation in WSL
Linux uses Copy-on-Write and fast duplication. Windows uses `CreateProcess()` therefore, WSL must simulate `fork()` using NT mechanisms → slower.

## Filesystem Management in WSL1
WSL1 stores Linux filesystem data inside:
```
%LOCALAPPDATA%\Packages\<Distro>\LocalState\rootfs
```
**Pros:** Fast access to Windows files (`/mnt/c`).

**Cons:** Slow Linux FS operations because NTFS isn't optimized for Linux semantics.
