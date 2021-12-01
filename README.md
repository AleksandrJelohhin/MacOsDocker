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

- WSL2 feature for windows
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
- Virtual Machine Platform
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
3) Download and Install Linux kernel update package
```
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```
4) Set Default version for WSL = 2
```
wsl --set-default-version 2
```
5) Donwload and Intall Linux sub system as WSL **(Ubuntu in my case)**
```
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing
```
```
curl.exe -L -o ubuntu-2004.appx https://aka.ms/wsl-ubuntu-2004
```
```
Add-AppxPackage .\Ubuntu.appx
```
6) Start WSL Linux, you've installed and create new user with root password
7) Open Docker Desktop and Enable WSL2 suport for your WSL Linux. Docker information can be found here
```
https://docs.docker.com/desktop/windows/wsl/
```
### Linux Machine
1) Login as root
```
sudo su -
```
2) Update Linux Distro (Ubuntu was used in my case)
```
sudo apt update && sudo apt -y upgrade
```
3) Install all packages need for virtualization
```
sudo apt -y install build-essential libncurses-dev bison flex libssl-dev libelf-dev cpu-checker qemu-kvm aria2 dwarves qemu-system qemu-utils python3 python3-pip
```
4) Download latest WSL2 Linux Kernel
```
aria2c -x 10 https://github.com/microsoft/WSL2-Linux-Kernel/archive/linux-msft-wsl-5.10.74.3.tar.gz
tar -xf WSL2-Linux-Kernel-linux-msft-wsl-5.10.74.3.tar.gz
cd WSL2-Linux-Kernel-linux-msft-wsl-5.10.74.3
```
