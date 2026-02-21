# WSL Registry Internals

All WSL distribution metadata is stored under:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Lxss
```

![wsl_registry_internals](../../../../.attachments/WSL_registry_internals.png)

## Per-Distro Metadaata

Each distribution entry contains:
- `BasePath` - Path to the VHDX
- `DistributionName`
- `Version` (WSL 1 or 2)
- Unique GUID
