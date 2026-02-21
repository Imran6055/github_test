# Filesystem Interoperability and 9P

## `/mnt/c` and DrvFs
- Windows drives are mounted using **DrvFs**
- Drvfs is a FUSE-like filesystem, not a kernel filesystem

## \\wsl$ UNC Path
Windows access Linux files using UNC(Univeral Naming Convention)

```
\\wsl$\<DistoName>\
```

## 9P Fileserver

WSL2 runs a 9P (Plan 9) **protocol server** inside the VM. They represent windows, network connections, processes, and almost anything else available in the operating system.
This allows:
* Widows Explorer access
* VS Code remote integration
* Windows application access to Linux files

## Important Note
9P is the primary reason Windows > Linux file operations are slower in WSL2.

# Case Sensitivity and Symlinks
- Case sensitivity is emulated
- Windows-reserved filenames (`con`, `aux`) can cause issues
- Symlink behavior does not perfectly match native Linux
