# pytorch-amd-gpu

# 📖 Introduction

After having some trouble installing and using PyTorch on my AMD GPU I wanted to share the process that worked for me. I am using a Radeon RX 6800 XT. But all GPUs with RDNA2 and RDNA3 should work perfectly fine.

# 🔧 How-To
## Prequesites

- Linux Ubuntu 22.04 LTS (v23.10 did NOT work for me). NOTE: When using multiple operating systems on your comupter I suggest you try to install Linux without a dual boot setup. There are several guides on the Internet on how to do this. The simplest way without messing anything up with your other operating systems, like it is often the case for example with Windows, I recommend you disconnect all your drives (HDD, SSD, M.2, ...) except the formated drive (internal or external) and then plugin your installation usb and install Ubuntu. After the installation is finished you can power off your computer completely and connect your harddrives again. NOTE: To prevent damage on your computer and yourself always make sure to disconnect the powercable completely, when opening your computer ;) <br>

-  `` sudo apt update && sudo apt install -y git python3-pip python3-venv python3-dev libstdc++-12-dev ``<br>
  
- disable Secure Boot in your BIOS, I had trouble entering MOK keys when trying to install the AMD Drivers

## Rocm Installation
- driver with rocm support:<br>

  ``curl -O https://repo.radeon.com/amdgpu-install/5.7.1/ubuntu/jammy/amdgpu-install_5.7.50701-1_all.deb``<br>

  ``sudo dpkg -i amdgpu-install_5.7.50701-1_all.deb ``<br>

  ``sudo amdgpu-install --usecase=graphics,rocm``<br>
- Modify user account settings:<br>

  ``sudo usermod -aG video $USER``<br>


  ``sudo usermod -aG render $USER``<br>
- reboot for changes to take affect (sudo reboot)<br>

Now rocm with the corresponing AMD gpu driver should be installed

## Get Pytorch

The next step is getting the right PyTorch version. I used the current stable version for rocm 5.7 (that is why we installed the 5.7 drivers instead of the 6.0 drivers earlier, because 6.0 doesn't seem to be backwards compatible with 5.x). Note: I use [Conda](https://www.anaconda.com/download) for my python environments, but other virtual environments should work aswell:

`` pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm5.7 ``

# 🐛 Troubleshooting

I also had a problem with PyTorch also recognizing my internal cpu gpu from my AMD RYZEN 7 7800x3D. I fixed this by modifing the bashrc, to tell the system what my actual target cpu is. If you get a similar error try adding this line to your bashsrc by the following steps:

1) ``nano ~/.bashrc``

2) ``export HIP_VISIBLE_DEVICES=0`` (add this line to bassrc, save and exit)

Now you are set and you should be able to run PyTorch computation on your GPU. To test this you can execute the [script](https://github.com/WobiWanKenobi/pytorch-amd-gpu/blob/main/test_your_rocm.py) in this repository. Or what I also did was testing it on a small CIFAR10 dataset neural network, to see if my GPU is really being utilised by PyTorch: https://pypi.org/project/test-pytorch-gpu/
 

