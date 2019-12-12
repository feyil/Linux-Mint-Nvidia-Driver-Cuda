## Linux Mint 19.2 Cinnamon NVIDIA 940M Driver Installation & CUDA 10.2 Toolkit Installation Instructions

* This repo includes set of steps to successfully install NVIDIA Driver and CUDA 10.2 in Mint 19.2 Cinnamon

### Official Driver Installation for NVIDIA 940M


1. First make the environment ready for driver installation by running below commands:

```bash
$ sudo dpkg --add-architecture i386
$ sudo apt update
$ sudo apt install build-essential libc6:i386
```

2. Second dowload offical NVIDIA driver from NVIDIA driver websites. It is a .run file which includes necessary scripts to install driver.

3. Disable Nouveau Nvidia driver

```bash
$ sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
$ sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```

4. Confirm Updated files has following contents

```bash
$ cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
blacklist nouveau
options nouveau modeset=0
```

5. Enter the following linux command to regenerate initramfs:

```bash
$ sudo update-initramfs -u
```

6. Reboot the system when the computer boots up Nouveau Driver has been disabled.

```bash
$ sudo reboot
```

7. Stop Desktop Manager. After executing the following linux command the display server will stop, therefore make sure you save all your current work ( if any ) before you proceed:

```bash
$ sudo telinit 3
```

8. Hit CTRL+ALT+F1 and login with your username and password to open a new TTY1 session or perform the Nvidia driver installation via SSH bash.

9. Install Nvidia Driver

```bash
$ sudo bash NVIDIA-Linux-x86_64-xxx.xx.run
```
    - Accept License
    - Would you like to register the kernel module sources with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later. -> **YES**
    - Install NVIDIA's 32-bit compatibility libraries? -> **YES**
    - The distribution-provided pre-install script failed! Are you sure you want to continue? -> CONTINUE INSTALLATION
    An incomplete installation of libglvnd was found. Do you want to install a full copy of libglvnd? This will overwrite any existing libglvnd libraries. -> **Install and overwrite existing files**
    - Would you like to run the nvidia-xconfig utility? -> **YES**

10. NVIDIA Driver is now installed. Reboot the system

```bash
$ sudo reboot
```

11. After NVIDIA Driver installation when the splash screen pop-up. Screen will go black hit CTRL+ALT+F1 and login with username and password

12. Initialize a new X session for the system. GUI Interface will open.

```bash
$ startx
```

13. So, we are back to the GUI now we know that our GPU is NVIDIA Optimus capable so will install nvidia-prime

```bash
$ sudo apt-get install nvidia-prime
```

14. Reboot the system when the reboot succeed GUI will appear without typing **startx** or following step **12**

15. Thats it we successfully installed latest Nvidia offical driver to the system. You can configure NVIDIA settings from NVIDIA X Server


### CUDA 10.2 Installation

1. Go to the NVIDIA Offical website and dowload CUDA 10.2 Toolkit with following instruction for .deb installation method

```bash
$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
$ sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
$ wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
$ sudo dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub
$ sudo apt-get update
$ sudo apt-get -y install cuda
```

2. After the succesfull completation of cuda reboot the system

```bash
$ sudo reboot
```

3. If nothing goes wrong we have succesfully installed. Lets check our compiler version

```bash
$ cd /etc/usr/local/cuda-10-2/bin
$ ./nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Oct_23_19:24:38_PDT_2019
Cuda compilation tools, release 10.2, V10.2.89
```
4. Lets test our compiler with NVDIA's examples:

```bash
$ cd /etc/usr/local/cuda/samples/0_Simple/matrixMul
$ make
$ ./matrxiMul
[Matrix Multiply Using CUDA] - Starting...
GPU Device 0: "Maxwell" with compute capability 5.0

MatrixA(320,320), MatrixB(640,320)
Computing result using CUDA Kernel...
done
Performance= 105.24 GFlop/s, Time= 1.245 msec, Size= 131072000 Ops, WorkgroupSize= 1024 threads/block
Checking computed result for correctness: Result = PASS

NOTE: The CUDA Samples are not meant for performancemeasurements. Results may vary when GPU Boost is enabled.
```

5. Lets configure our environment variables to access CUDA compiler in the system wide. Add followings to the end of **.bashrc** file.

```bash
export PATH=/usr/local/cuda-10.2/bin${PATH:+:${PATH}}$
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

6. Open a new terminal window and type:

```bash
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Oct_23_19:24:38_PDT_2019
Cuda compilation tools, release 10.2, V10.2.89
```

7. That's it we successfully installed CUDA 10.2 Toolkit we can compile our programs with nvcc command.

## References

[How to install Nvidia drivers on Linux Mint](https://linuxconfig.org/how-to-install-nvidia-drivers-on-linux-mint)

[How to disable Nouveau nvidia driver on Ubuntu 18.04 Bionic Beaver Linux](https://linuxconfig.org/how-to-disable-nouveau-nvidia-driver-on-ubuntu-18-04-bionic-beaver-linux)

[How to install CUDA 9.2 on Ubuntu 18.04](https://www.pugetsystems.com/labs/hpc/How-to-install-CUDA-9-2-on-Ubuntu-18-04-1184/?__cf_chl_captcha_tk__=1511c5fe74263a3798c679f70d04769050550431-1576167126-0-Aaih6ZtS2d60mfEjzHn6xzNKD4cjewqq2shFZETVRi6R5GiXMAGcsapIkV_gDPcm98rAfUlUrOuhDapITn2PQlXmC3m5ZrhKPP8hhu499fi9-nHMn0oXrHS__Ewo-2Vl_eyILO_g7lI98s2PSWu-vPTVU7GxdfEiHl7FpaXGvktlGCYma_N0kLSA1W1L5ESfnKgvxToewKMiMJJh_iwIJKnJgZ1Co7XZqcoMpJix137NWD-z10vTg97_0QDFC1dKoXCfkK5bCXIOBAknlMWMdMOytWs0J650phh9LdF9v-w2k0s74_ePSZyqGtH1Lwyr__hMGIrUvdAipLrCBv3_3-lJ-BWoYkaiDcSWz_eYYZ_j8KmRiEhKUlWpESjZMKAkVg)

[nvcc error, “no command” verifying installation of CUDA 10 on Debian 9 Stretch](https://unix.stackexchange.com/questions/480855/nvcc-error-no-command-verifying-installation-of-cuda-10-on-debian-9-stretch)