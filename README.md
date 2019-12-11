## Linux Mint 19.2 Cinnamon NVIDIA 940M Driver Installation & CUDA 10.2 Toolkit Installation Instructions

* This repo includes set of steps to successfully install NVIDIA Driver and CUDA 10.2 in Mint 19.2 Cinnamon

### Official Driver Installation for NVIDIA 940M


* First make the environment ready for driver installation by running below commands:

```shell
$ sudo dpkg --add-architecture i386
$ sudo apt update
$ sudo apt install build-essential libc6:i386
```



* Second dowload offical NVIDIA driver from NVIDIA driver web sites. It is a .run file which includes necessary scripts to install driver.

