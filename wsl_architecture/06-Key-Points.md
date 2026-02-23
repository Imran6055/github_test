## Points to Remember:

1. Pico processes are not used anywhere else in Windows
* They were invented solely to allow Linux binaries to run as first‑class Windows processes.

2. WSL 1 doesn’t use virtualization at all
* Many assume it is a small VM, but WSL 1 is closer to a Windows syscall like Wine (in reverse).

3. WSL 2 kernel is built and shipped by Microsoft—not by your Linux distro
* It’s updated via Windows Update and is uniform across all distros.

4. Cross‑OS file access (`/mnt/c` vs `\wsl$`) behaves fundamentally differently
* `mnt/c` uses DrvFS (slow for Linux workloads).
* `\\wsl$` uses 9P protocol inside the VM and these choices dramatically change performance.