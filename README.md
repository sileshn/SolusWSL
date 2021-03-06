# SolusWSL
Solus on WSL2 (Windows 10 FCU or later) based on [wsldl](https://github.com/yuk7/wsldl)

[![Screenshot-2021-02-04-074402.png](https://i.postimg.cc/Df1gxs31/Screenshot-2021-02-04-074402.png)](https://postimg.cc/mh2CDPDr)
[![Github All Releases](https://img.shields.io/github/downloads/sileshn/SolusWSL/total.svg?style=flat-square)](https://github.com/sileshn/SolusWSL/releases) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com) ![License](https://img.shields.io/github/license/yosukes-dev/FedoraWSL.svg?style=flat-square)

## Requirements
* For x64 systems: Version 1903 or higher, with Build 18362 or higher.
* For ARM64 systems: Version 2004 or higher, with Build 19041 or higher.
* Builds lower than 18362 do not support WSL 2.
* Enable Windows Subsystem for Linux feature.
```cmd
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
* Enable Virtual Machine feature
```cmd
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
* Download and install the Linux kernel update package from [here](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).

For more details, check [this](https://docs.microsoft.com/en-us/windows/wsl/install-win10) microsoft document.

## How to install
* Make sure all the steps mentioned under "Requirements" are completed.
* Set wsl2 as default. Run the command below in a windows cmd terminal.
```dos
wsl --set-default-version 2
```
* [Download](https://github.com/sileshn/SolusWSL/releases/latest) installer zip
* Extract all files in zip file to same directory.
* Run Manjaro.exe to Extract rootfs and Register to WSL

**Note:**
Exe filename is using the instance name to register. If you rename it you can register with a diffrent name and have multiple installs.

## How to setup
Open Solus.exe and run the following commands.
```dos
passwd
sed -i 's#\# %wheel ALL=(ALL) ALL#%wheel ALL=(ALL) ALL#g' /etc/sudoers
useradd -m -G wheel -s /bin/bash <username>
passwd <username>
exit
```
Execute the command below in a windows cmd terminal from the directory where Solus.exe is installed.
```dos
>Solus.exe config --default-user <username>
```

## How to use installed instance
#### exe Usage
```
Usage :
    <no args>
      - Open a new shell with your default settings.

    run <command line>
      - Run the given command line in that distro. Inherit current directory.

    runp <command line (includes windows path)>
      - Run the path translated command line in that distro.

    config [setting [value]]
      - `--default-user <user>`: Set the default user for this distro to <user>
      - `--default-uid <uid>`: Set the default user uid for this distro to <uid>
      - `--append-path <on|off>`: Switch of Append Windows PATH to $PATH
      - `--mount-drive <on|off>`: Switch of Mount drives
      - `--default-term <default|wt|flute>`: Set default terminal window

    get [setting]
      - `--default-uid`: Get the default user uid in this distro
      - `--append-path`: Get on/off status of Append Windows PATH to $PATH
      - `--mount-drive`: Get on/off status of Mount drives
      - `--wsl-version`: Get WSL Version 1/2 for this distro
      - `--default-term`: Get Default Terminal for this distro launcher
      - `--lxguid`: Get WSL GUID key for this distro

    backup [contents]
      - `--tgz`: Output backup.tar.gz to the current directory using tar command
      - `--reg`: Output settings registry file to the current directory

    clean
      - Uninstall the distro.

    help
      - Print this usage message.
```

#### Just Run exe
```cmd
>{InstanceName}.exe
[root@PC-NAME user]#
```

#### Run with command line
```cmd
>{InstanceName}.exe run uname -r
4.4.0-43-Microsoft
```

#### Run with command line with path translation
```cmd
>{InstanceName}.exe runp echo C:\Windows\System32\cmd.exe
/mnt/c/Windows/System32/cmd.exe
```

#### Change Default User(id command required)
```cmd
>{InstanceName}.exe config --default-user user

>{InstanceName}.exe
[user@PC-NAME dir]$
```

#### Set "Windows Terminal" as default terminal
```cmd
>{InstanceName}.exe config --default-term wt
```

## How to uninstall instance
```dos
>Solus.exe clean

```

## How to build
#### Prerequisites

Docker, tar, zip, unzip need to be installed.

If you want to build using solus unstable profile, checkout the unstable branch.
```dos
git clone git@gitlab.com:sileshn/SolusWSL.git
cd SolusWSL
make

```
Copy the Solus-main.zip file to a safe location and run the command below to clean.
```dos
make clean

```

## How to install & run docker in SolusWSL without using docker desktop
Install docker binaries. Note that installing docker from solus repos doesn't work as the latest version in solus is 19.03.14. We need 20.10 versions and above.
```dos
sudo eopkg it -y wget
wget https://download.docker.com/linux/static/stable/x86_64/docker-20.10.5.tgz
tar xzvf docker-20.10.4.tgz
sudo cp docker/* /usr/bin/

```
You can now delete the downloaded files if needed.
```dos
rm -rf docker
rm docker-20.10.5.tgz

```

To manage docker as a non-root user, create and add user to docker group. 
```dos
sudo groupadd docker
sudo usermod -aG docker $USER

```

Follow [this](https://blog.nillsf.com/index.php/2020/06/29/how-to-automatically-start-the-docker-daemon-on-wsl2/) blog post for further setup instructions.

[![Screenshot-2021-02-18-120518.png](https://i.postimg.cc/7YqVpBqP/Screenshot-2021-02-18-120518.png)](https://postimg.cc/V5HntWS2)
