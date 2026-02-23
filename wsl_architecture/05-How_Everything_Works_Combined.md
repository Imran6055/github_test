How These Components Work Together (BackGround Flow)

1. User runs `wsl.exe` or opens a Linux terminal
2. wslservice.exe receives a CreateInstance() request from the client
3. wslservice.exe consults registry → determines if WSL1 or WSL2
4. For WSL1, LXCore drivers load and a pico process is spawned
5. For WSL2, wslservice.exe uses HCS to start the lightweight Linux VM
6. Once booted, mini_init runs inside the VM to initialize user mode
7. LxssManager tracks the new instance and associates processes
8. wslhost.exe facilitates pipes, IO, filesystem, and networking
9. The Linux shell (bash, zsh, etc.) connects via hvSockets
10. Background daemons (e.g., systemd, if enabled) continue running until last client exits