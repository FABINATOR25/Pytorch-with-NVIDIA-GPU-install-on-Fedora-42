# Pytorch with NVIDIA GPU install on Fedora 42

> This guide documents how to set up PyTorch with an NVIDIA GPU on Fedora 42.  
> I created it because I found limited, outdated, or incomplete instructions online, so this tutorial consolidates an easy, clean, working method.

Tested with : 
 1. Dual boot Window/Fedora 
 2. Kernel version : 6.16.12
 3. GPU : Nvidia 1660Ti laptop

By default, Fedora comes with preinstalled Open-source Vulkan Driver.

You can check by going to : 

**Settings -> System -> About -> System Details -> Graphic**



## Step 1: Install NVIDIA Driver

First, let’s install the proprietary NVIDIA drivers.

1. Open **Software**.
2. Scroll down to **Hardware Drivers**.
3. Install **NVIDIA Linux Graphics Driver**.
4. Reboot your computer.

Note: The first boot after installation might take a little longer.

## Step 2 : CUDA toolkit
Now if you go back to **Settings -> System -> About -> System Details -> Graphic**, you can see that it as change.

Next, follow the CUDA installation instructions for Fedora 42 from the official NVIDIA repository.

📄 **Reference:** https://rpmfusion.org/Howto/CUDA

For Fedora 42 it says : 

```bash
sudo dnf config-manager addrepo --from-repofile=https://developer.download.nvidia.com/compute/cuda/repos/fedora42/$(uname -m)/cuda-fedora42.repo

sudo dnf clean all

sudo dnf config-manager setopt cuda-fedora42-$(uname -m).exclude=nvidia-driver,nvidia-modprobe,nvidia-persistenced,nvidia-settings,nvidia-libXNVCtrl,nvidia-xconfig

sudo dnf -y install cuda-toolkit xorg-x11-drv-nvidia-cuda
```
## STEP 3 : Find your CUDA version

Now you can use the command **nvidia-smi** in the terminal.
You will be able to see your driver version and your CUDA Version (mine is 13.0)
Note your CUDA version.

<img width="740" height="342" alt="annotely_image" src="https://github.com/user-attachments/assets/114d7899-b5e6-4106-862f-669a7633c16c" />


## STEP 4 : Install Pytorch
Now go on the pytorch site :https://pytorch.org/get-started/locally/
and select the CUDA version that corresponds to your system. 

For CUDA 13 using Pip it will be : 
```bash
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

```
> ⚠️ This installs PyTorch system-wide. If you’d rather keep your setup clean, use a virtual environment instead.

Now with your terminal : 

```bash
python3

import torch
torch.cuda.is_available()
```
This should return True

Done !

## Optional : Double check
 
Lets double check by sending tensor on your graphic card : 

```
import torch
a = torch.rand(2,2) # creating an 2 by 2 tensor
a.to(0) # sending it on GPU
```
Open a terminal run nvidia-smi, under processes you should see "python3"
