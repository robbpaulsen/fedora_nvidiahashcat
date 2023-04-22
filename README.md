# Installation of Nvidia drivers Cuda and Hashcat on Fedora 37-ish

## Faster downloads with DNF

Edit with your favorite text editor the file `/etc/dnf/dnf.conf`, you will add this at the end of the last line:

```sh

[main] 
gpgcheck=1 
installonly_limit=3 
clean_requirements_on_remove=True 
best=False 
skip_if_unavailable=True 
fastestmirror=1
max_parallel_downloads=10 
deltarpm=true
```
Save file and  exit.

## First thing enable RPM fusion non-free repository

```sh

* sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
## Update & Reboot
```sh

sudo dnf -y upgrade --refresh
```

## Now fedora has a hardware firmware detection and installer for users, install alla missing hardware drivers:

```sh

sudo fwupdmgr get-devices 
sudo fwupdmgr refresh --force 
sudo fwupdmgr get-updates 
sudo fwupdmgr update
```

## Nvidia and Cuda Drivers Installation

```sh

sudo dnf install -y akmod-nvidia
sudo dnf install -y xorg-x11-drv-nvidia-cuda
```

# VERY VERY IMPORTANT, AFTER INSTALLATION, LEAVE YOUR MACHINE TO REST FOR AT LEAST 5 MINUTES AND IN PERFECT SCENARIO A 10 MINUTE REST. GPU DRIVERS DONT SHOW RIGHT AWAY AFTER COMPILATION, SO DO NOT REBOOT RIGHTAWAY THIS LEADS TO BLANK SCREENS OR BLACK SCREENS AND/OR KERNEL PANICS.

After Reboot, check the compiled drivers with:

```sh

modinfo -F version nvidia
nvidia-smi
```

## Hashcat Installation

After reboot create a "src" directory just for hashcat source tar ball file, being today the 28 of Februrary the latest source tar ball is Version 6.2.

```
wget https://hashcat.net/files/hashcat-6.2.6.tar.gz
```

Use 7zip to extract it:

```
7z x hashcat-6.2.6.tar.gz
7z x hashcat-6.2.6.tar
cd hashcat-6.2.6
```

## Building Hashcat

to build hashcat is plain straigh forward with make commands

```
make
sudo make install
```
## Tunning Hashcat to your GPU Card

Run the `hashcat -I` command depending on how many devices it pulls you out you will benchmark hashcat and all the devices that it recognize to set them for password cracking so if it enumarates 1 device you benchmark like this:

```
hashcat -b -d 1 | uniq

```

If it enumerates 2 you add like this:

```
hashcat -b -d 1,2 | uniq
```

If its three:

```
hashcat -b -d 1,2,3 | uniq
```

You get the idea form there.