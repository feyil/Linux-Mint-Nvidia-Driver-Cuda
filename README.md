## Linux Mint 19.2 Cinnamon NVIDIA 940M Driver Installation & CUDA 10.2 Toolkit Installation Instructions

* This repo includes set of steps to successfully install NVIDIA Driver and CUDA 10.2 in Mint 19.2 Cinnamon

### Official Driver Installation for NVIDIA 940M


1. First make the environment ready for driver installation by running below commands:

```shell
$ sudo dpkg --add-architecture i386
$ sudo apt update
$ sudo apt install build-essential libc6:i386
```

2. Second dowload offical NVIDIA driver from NVIDIA driver websites. It is a .run file which includes necessary scripts to install driver.

3. Disable Nouveau Nvidia driver

```shell
$ sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
$ sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```

4. Confirm Updated files has following contents

```shell
$ cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
blacklist nouveau
options nouveau modeset=0
```

5. Enter the following linux command to regenerate initramfs:

```shell
$ sudo update-initramfs -u
```

6. Reboot the system when the computer boots up Nouveau Driver has been disabled.

```shell
$ sudo reboot
```

7. Stop Desktop Manager. After executing the following linux command the display server will stop, therefore make sure you save all your current work ( if any ) before you proceed:

```shell
$ sudo telinit 3
```

8. Hit CTRL+ALT+F1 and login with your username and password to open a new TTY1 session or perform the Nvidia driver installation via SSH shell.

9. Install Nvidia Driver

```shell
$ sudo bash NVIDIA-Linux-x86_64-xxx.xx.run
```
    - Accept License
    - Would you like to register the kernel module sources with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later. -> **YES**
    - Install NVIDIA's 32-bit compatibility libraries? -> **YES**
    - The distribution-provided pre-install script failed! Are you sure you want to continue? -> CONTINUE INSTALLATION
    An incomplete installation of libglvnd was found. Do you want to install a full copy of libglvnd? This will overwrite any existing libglvnd libraries. -> **Install and overwrite existing files**
    - Would you like to run the nvidia-xconfig utility? -> **YES**

10. NVIDIA Driver is now installed. Reboot the system

```shell
$ sudo reboot
```

11. After NVIDIA Driver installation when the splash screen pop-up. Screen will go black hit CTRL+ALT+F1 and login with username and password

12. Initialize a new X session for the system. GUI Interface will open.

```shell
$ startx
```

13. So, where back to the GUI now we know that our GPU is NVIDIA Optimus capable so will install nvidia-prime

```shell
$ sudo apt-get install nvidia-prime
```

14. Reboot the system when the reboot succeed GUI will appear without typing **startx** or following step **12**

15. Thats it we successfully installed latest nvidia offical driver to the system. You can configure NVIDIA setting from NVIDIA X Server

