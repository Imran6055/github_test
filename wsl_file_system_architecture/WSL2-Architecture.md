# WSL2 File System Architecture (Lightweight VM)

- Stores Linux files inside a native ext4 filesystem within `ext4.vhdx` per distro.

Default Location:

```
%LOCALAPPDATA%\Packages\<DistroPackage>\LocalState\ext4.vhdx
```

# Filesystem Layout(same like linuxfs):
The ext4 filesystem contains:
* `/home`
* `/etc`
* `/usr`
* `/var`
* `/root`

# VHDX Characteristics 
* Dynammically expands
* Default maximum size is approximately 1 TB
* Does not shrink automatically after the file deletion


