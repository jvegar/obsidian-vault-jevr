# How to compact a VHD file

Be sure to stop your WSL distro first.
```sh
wsl.exe --shutdown <WSL distro name>
```

## Option 1: Using PowerShell Optimize-VHD
This option can only be used in Windows 11 Pro.

```sh
Optimize-VHD -Path <Path to VHD file>
```


## Option 2: Using Diskpart
This option is more general. It can be run from command line.

```sh
diskpart
select vdisk file="<Pathto VHD file>"
compact vdisk
exit
```

