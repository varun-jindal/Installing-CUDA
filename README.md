# GPU atomicity violations

This is an exploratory project to evaluate if prescence or abscence of data races can cause atomicity violations for GPUs. And if that is possible then come up with a method to detect that violation.

## Data races in CUDA GPUs

There are many papers for detecting data races in NVIDIA GPUs like [Barracuda](https://www.cs.uic.edu/~mansky/barracuda.pdf), [CURD](https://dl.acm.org/citation.cfm?doid=3192366.3192368). We had tested initially some programs from [rodinia3.1](http://lava.cs.virginia.edu/Rodinia/download_links.htm) with CUDA-Racecheck. Racecheck is a part of CUDA toolkit. Particularly CUDA "cuda_10.0.130_410.48_linux" was used on Ubuntu 16.04 machine.

## Installing CUDA from runfile

There are many ways for installing CUDA toolkit. I used runfile method. Following steps are simplied version of [official documentation](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

### Steps to follow:

#### 1.Pre-install steps: 
Make sure you have a Nvidia GPU
Add linux headers using command:
```bash
sudo apt-get install linux-headers-$(uname -r)
```
Download the image from [Nvidia DevZone](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604) page and verify with 
```bash
md5sum <file>
```

#### 2.Disabling nouveau:
Follow these commands:
```bash
cd /etc/modprobe.d
vim blacklist-nouveau.conf
```
Type following content in file:
```bash
blacklist nouveau
options nouveau modeset=0
```
Regenerate kernel initramfs:
```bash
sudo update-initramfs -u
```

#### 3.Boot into text mode
* While booting pause when GRUB menu appears and press 'e'
* locate line containig "linux  /vmlinuz ... ... ro" replace whatever is written after that usually "quiet splash" with 3 to temporarily enter text mode
* press Ctrl + X. It prompts to ask username(type exactly whatever username your pc has, eg: xyz-hp-pc) and password.

#### 4.Use the runfile
* Change directory to the one containg runfile.
* Use following commnd to run the setup:
```bash
sudo sh cuda_<>_linux.run
```
type in the runfile name
* Setup prompts after a while for EULA agreement. Press spacebar to reach end of EULA (arrow keys won't work).Type in accept and follow on-screen prompts
* Installationn path can be left to default.

#### 5.Return to graphical interface
Use following command:
```bash
sudo service lightdm restart
```
Wait for sometime it returns to graphical interface.
By now cuda installation is done. Restart once to make sure that any settings required are updated in system.

#### 6.Post-installation
* Check driver version
```bash
cat /proc/driver/nvidia/version
```
* Check CUDA toolkit version
```bash
nvcc -V
```
If both  give some prompts in return it means installation is successfully done.
Refer to section 7.3.1 of official documentation to add additional libraries.
