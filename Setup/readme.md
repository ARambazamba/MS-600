# Lab VM Setup Guide

This is a semin-automated guide for a Azure Lab VM Setup. Feel free to automate the missing steps on a seperate branch in your fork and create a pull request. An introduction video into forks and pull requests can be found [here](https://www.youtube.com/watch?v=nT8KGYVurIU)

In order for Docker to work on a Windows 10 VM you need to install Hyper-V or use Windows Subsystem for Linux 2 (WSL2). Therefore you must use hardware that supports [Nested Virtualization](https://docs.microsoft.com/en-us/azure/lab-services/how-to-enable-nested-virtualization-template-vm). A detailed Setup Guide can be found [here](https://github.com/ARambazamba/ClassSetup).

Script `create-lab-vm.azcli`:

```bash
rnd=$RANDOM
loc=westeurope
grp=ms600-lab
vmname=labvm-$rnd
user=ms600labadmin
pwd=Lab@dmin1234

az group create -n $grp -l $loc

az vm create -g $grp -n $vmname --admin-username $user --admin-password $pwd --image MicrosoftWindowsDesktop:Windows-10:21h1-pro-g2:19043.985.2105141120 --size Standard_E2s_v3

az vm auto-shutdown -g $grp -n $vmname --time 1830
```

Note: The image name of the Windows 10:20h2 image changes frequently. You might have to update the image name. Get a list of all Windows 10 images: `az vm image list -f "Windows-10" --location westeurope --all`. Remember to support WSL2 you must have at least patch level `20h2`

You could also execute `create-lab-vm.sh` or run the following remote script in Cloud Shell

```bash
curl https://raw.githubusercontent.com/arambazamba/ms-600/master/Setup/create-lab-vm.sh | bash
```

![create-labvm](_images/create-lab-vm.jpg)

## Connect to VM

Go to Ressource Group `ms600-lab` and connect to VM using RDP and the credentials that you have used in the script:

Download RDP File:

![download-rdp](_images/download-rdp.jpg)

Optional - Disable Login:

![disable-login](_images/disable-login.jpg)

Sign In & Remember:

![connect-rdp](_images/trust-vm.jpg)

Credentials:

```
user=azlabadmin
pwd=Lab@dmin1234
```

![sign-in.jpg](_images/sign-in.jpg)

Accept Settings:

![accept-settings](_images/accept-settings.jpg)

## Install Software

To install Software run the script `setup-az-204.ps1` from an elevated PowerShell prompt:

![run-as](_images/run-as.jpg)

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force;
Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/ARambazamba/AZ-204/master/Setup/setup-az-204.ps1'))
```

> Note: This script will run for approx 20 min. No need to wait! In the meantime you can continue to fork and clone my repo as described in the next section.

Congratulations you have completed the base setup of a labs vm!

---

## Fork & Clone Class Repo and set Github requirements

Set User and E-Mail in order to be able to commit to git:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@yourdomain.com"
```

Go to `https://github.com/ARambazamba/az-204` and fork the repo

![forking-wf](_images/fork.jpg)

The forking-workflow allows you to commit your changes to your fork of the repo and still get updates on the repo

![forking-wf](_images/forking-workflow.jpg)

Clone Class Repo:

```bash
git clone https://github.com/Student01/az-204
```

> Note: If you have forked the class repo clone your own fork, otherwise use https://github.com/ARambazamba/az-204

---

## Setup Docker Support

There are two options to configure Docker support. WSL2 recommended

### Option 1 - Install Docker with Hyper-V

Install Hyper-V using `setup-docker.ps1` and restart after execution.

Check Docker Settings:

![docker-test](_images/docker-settings.jpg)

---

### Option 2 - Install Docker with WSL2

#### <a id="wsl">Install WSL2</a>

Execute script `setup-wsl2.ps1` multible times to setup WSL2

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force;
Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/ARambazamba/AZ-204/master/Setup/setup-wsl2.ps1'))
```

First Run:

![finish-sw](_images/finish-sw.jpg)

Second Run:

![select-distro](_images/select-distro.jpg)

After the second run your selected linux distro will be opened an you can set your username:

> Note: You can take the same credntials just like on the Windows VM

```
user=azlabadmin
pwd=Lab@dmin1234
```

![ubuntu-setup](_images/ubuntu-setup.jpg)

#### <a id="docker-wsl">Install Docker using WSL2</a>

Install Docker Desktop using an elevated Powershell:

```
choco install docker-desktop -y
```

![install-docker-desktop](_images/install-docker-desktop.jpg)

Log off and on after installation:

![log-off](_images/log-off.jpg)

> Note: Enter `logoff` in the current console

Start Docker Desktop and switch to settings:

![switch-settings](_images/switch-settings.jpg)

Check the WSL2 settings:

![switch-settings](_images/docker-settings-wsl.jpg)

---

### Test Docker Installation

In the console window execute:

```
docker run hello-world
```

![docker-test](_images/docker-test.png)

## <a id="teams">Optional - Using Teams in VM</a>

To avoid switching between Host an VM you can use Microsoft Teams in the VM and use Audio Redirection

Edit RDP Connection:

![edit-rdp](_images/edit-rdp.jpg)

Redirect Audio:

![remote-audio](_images/remote-audio.jpg)

Use Secondary Display:

![all-monitors](_images/all-monitors.jpg)
