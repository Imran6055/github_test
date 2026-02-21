# VHDX Storage Behavior and Optimization

## Disk Growth Behavior
- VHDX files grow automatically
- Deleted files do not reclaim disk space

This is commonly misunderstood behavoir in WSL2.

# Required Prerequisites for Optimization

* WSL must be full shut down:
```
wsl --shutdown
```

## Optimize-VHD using powershell
In order to optimize ext4.vhd and clear unused spaces. You need to enable **Hyper-V management feature**, enabling Hyper-V automatically enables the feature on your system. If not enabled not, open PowerShell as an Administrator and run

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```
Once enabled, reboot the system to make the setting take effect.

After rebooting the system, open the powershell and type this command

```
Get-Command Optimize-vhd
```
![Optimize VHD](../../../../.attachments/Optimize_Vhd.png)

The following output confirms optimize-vhd enabled successfully.

Run optimization:

You need to run this command in powershell using Admin privilege:

```
Optimize-VHD -Path "C:\Users\<User>\AppData\Local\Packages\<DistroName>\LocalState\ext4.vhdx" -Mode Full
```

![Optimaize_VHD](../../../../.attachments/Optimize_vhd_command.png)


Optimize-VHD -Path <ext4.vhdx> -Mode Full

## Mode Explanation
* `Full` - Maximum space reclamation
* `Quick` - Minimal cleanup


## Export and Importing distro (Most effective to reclaim unused spaces)

### Prerequisites 
* Windows Subsystem for Linux
* Enough free disk space to store the exported archive

## Step1: Listen installed WSL Distributions
```
wsl --list --verbose
```
This commands will display all the installed distros in your system. Note the exact distro name you want to export. Make sure wsl not running.

## Step2: Export the Distro

Export the distribution to a `.tar` file

```
wsl --export ArchLinux C:\temp\Arch-backup.tar
```

Select the name of distro that  you want to import and path as you prefer, for example **wsl --export <distro_name> <path> <tarfilename>**

* This creates a compact archive
* Unused disk blocks are discarded during export

## Step 3: Import the Distro Back

```
wsl --import ArchLinux C:\WSL\ArchLinux C:\temp\Arch-backup.tar
```

* A new ext4.vhdx file is created
* The disk size reflects only the actual data in use


