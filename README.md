# MacOsDocker
Create MacOS Virtual Machine on Ubuntu using WSL2 tech

## For stable Windows 10 (I have not tested this option, but it defently must work)
1) You need to install Linux distro in Hyper-V
2) Enable guest virualization for installed disto using command: 
```
Set-VMProcessor -VMName <Your_Hyper-V_Machine_Name> -ExposeVirtualizationExtensions $true
```
Example
```
Set-VMProcessor -VMName "Ubuntu 20.04" -ExposeVirtualizationExtensions $true
```
3) Update Linux Distro (Ubuntu was used in my case)
```
sudo apt update && sudo apt -y upgrade
```
4) Install all packages need for virtualization **(Some of them may be not need anymore)**
```
sudo apt -y install build-essential libncurses-dev bison flex libssl-dev libelf-dev cpu-checker qemu-kvm aria2 dwarves qemu-system qemu-utils python3 python3-pip
```

## For Windows insider build of Windows 10 or Windows 11
### Windows Machine
1) Install Docker Desktop 
```
https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe
```
2) Open Powershell and enable: 
WSL2 feature for windows
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
Virtual Machine Platform
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
