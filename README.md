# Installation of Nvidia drivers Cuda and Hashcat on Fedora 37-ish

## First thing enable RPM fusion non-free repository

```
* sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

## Create a blacklist conf file for the nouveau community driver 

```
sudo vi /etc/modprobe.d/blacklist.conf

### add this to the file:
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```

## Next Steps:

```
* sudo dnf update -y
* sudo dnf install akmod-nvidia    # For rhel/centos users you need to use kmod-nvidia instead
* sudo dnf install xorg-x11-drv-nvidia
* sudo dnf install xorg-x11-drv-nvidia-cuda
```

## Giving time 

After the above installations and repository enabling, you need to give your Fedora machine about 5 to 10 minutes to settle and end the transaction, if you cold reboot on finished no bad thing will happen but you'll experience a couple of slow boots, so give it a 10 minute to settle and after that reboot the correct way, but first confirm installation with this:

```
nvidia-smi
```

After that you can reboot the machine:

```
shutdown --reboot
```

This way the system correctly prepare all the system wide for a whole reboot and it will take 60 seconds to perform it.
