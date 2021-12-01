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
5) Lets Edit Kernel
```
cp Microsoft/config-wsl .config
make menuconfig
```
6) Proceed to **Processor type and features** -> **Linux guest support** enable built-in **KVM guest support**
7) Build the new kernel (Will take some time)
```
make -j 8
```
8) Install modules (May be not needed but I've installed it anyway)
```
sudo make modules_install
```
9) Copy created Kernel to some folder you have access to (user folder for example)
```
cp arch/x86/boot/bzImage /mnt/c/Users/**<username>**/bzImage
nano /mnt/c/Users/**<username>**/.wslconfig
```
10) Paste but don't forget to change <username> to your Windows username:
```
[wsl2]
nestedVirtualization=true
kernel=C:\\Users\\<username>\\bzImage
pageReporting=true
kernelCommandLine=intel_iommu=on iommu=pt kvm.ignore_msrs=1 kvm-intel.nested=1 kvm-intel.ept=1 kvm-intel.emulate_invalid_guest_state=0 kvm-intel.enable_shadow_vmcs=1 kvm-intel.enable_apicv=1
```
11) Switch back to Windows and open Powershell and shutdown WSL Linux
```
wsl.exe --shutdown Ubuntu
```
12) **Restart Docker Daemon** 
13) Open WSL LINUX and check that new kernel is applied - **should show today's date and the time should be a few minutes ago**
```
uname -ar
```
If kernel is not updated and you used user folder as me, check that launched WSL Linux using that user 
14) Configure kvm-intel 
```
sudo nano /etc/modprobe.d/kvm-nested.conf
```
Paste:
```
options kvm-intel nested=1
options kvm-intel enable_shadow_vmcs=1
options kvm-intel enable_apicv=1
options kvm-intel ept=1
```
15) Load kenel module
```
sudo modprobe kvm_intel
```
16) Check KVM is OK - **Must say that everything is OK**, otherwise you wont be able to run guest virtualization 
```
kvm-ok
```
17) Switch back to Windows.
Download and Install some "XServer" app that can run graphical Linux apps on Windows.
I was using GWSL.
```
https://opticos.github.io/gwsl/tutorials/download.html
```
18) Launch installed GWSL and Enable **GWSL Dsitro Tools** --> **Display/Audio Auto-Exporting**
![image](https://user-images.githubusercontent.com/2877844/144253178-62924a3e-11f1-4047-9ade-977656de18b0.png)
