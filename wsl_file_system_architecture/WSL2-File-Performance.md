# WSL2 File Performance Characteristics

## Native Linux Filesystem (ext4.vhdx)

* Faster I/O operations
* Near-native linux performance
* Recommended for builds and compilation

## Windows Filesystem Access (/mnt/c)
- Slower due to 9P protocoal overhead
- Accessing windows files from Linux is slow than WSL1 

## Performance Summary
- WSL1 faster for Windows file access, since it uses translation layer
- WSL2 faster for Linux filesystem operations
- Compiling large projects is faster inside ext4.vdhx
