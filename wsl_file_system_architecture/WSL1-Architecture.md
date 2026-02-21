# WSL1 File System Architecture (Translation Layer)

WSL1 uses a translation layer instead of a virtual machine.
Linux system calls are translated into Windows system calls.


## Storage Model
- Files stored directly on NTFS
- No virtual disk
- Default path:
`%LOCALAPPDATA%\lxss\`

## How WSL1 Emulates Linux Filesystem
WSL1 does not use ext4.
It simulates Linux using:

- NTFS + metadata translation
- A kernel driver: `lxcore.sys`
- A user‑space manager: `lxssmanager.dll`

WSL1 stores Linux permissions using NTFS extended attributes.
Windows attributes like: `LXATTRIB, LXUID, LXGID` are hidden NTFS metadata blobs that represent:

* `chmod`
* `chown`
* timestamps
* symlink types

### Known Limitations 
* Sysmlink behavior may break sometimes
* `chmod+x` does not behave exactly like ext4
* Extracting tarballs may lose metadata
