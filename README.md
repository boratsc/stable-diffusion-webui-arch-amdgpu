# stable-diffusion-webui-arch-amdgpu

How to install ROCm on ANY form of archlinux or archlinux variant

to start:

only archlinux:

 sudo pacman -Syyu

only on manjaro:

 sudo pacman-mirrors -f && sudo pacman -Syyu

 OR
   
 sudo pamac upgrade -a

Install yay 

 sudo pacman -S yay

Install ROCm

 yay -S git python-virtualenv wget python-pip dpkg make rocm-hip-sdk rocm-opencl-sdk gperftools bc pyenv

Add your user to the render group

 sudo gpasswd -d $USER render
 sudo gpasswd -d $USER video

Edit ~/.profile with your editor of choice.

 sudo nano ~/.profile

OR

 sudo nano /etc/environment

The difference? .profile does the current profile, where as environment does it on bootup, make sure to remove the "export" part if putting in environment.

Paste the following line at the bottom of the file, then press ctrl-x and save the file before exiting.

export HSA_OVERRIDE_GFX_VERSION=10.3.0

(change the version number to 11.0.0 for 7000 series)


Reboot your system

 reboot

after reboot

Next step is install python 3.10.6

 pyenv install 3.10.6

That command install python3.10.6 on your home dir, go to this dir

 cd /home/$USER/.pyenv/versions/3.10.6/bin/
 python -m venv venv
 . venv/bin/activate

Prompt should change to (venv)
 
We want to seperate these two commands so that it will install in necessary order for it to install "WHL"

 ./pip install wheel

THEN

 ./pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm5.7

Let's verify PyTorch was installed correctly with GPU support, so lets first enter the Python console.

 ./python3

Now enter the following two lines of code. If it returns True then everything was installed correctly.

 import torch

 torch.cuda.is_available()

If you see true - your gpu is working correctly.

Then enter exit() to exit the Python console.


Do this in either home folder or you can do:

 cd 
 git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
 cd stable-diffusion-webui
 nano webui-user.sh

Change this line in webui-user.sh to this:

python_cmd="/home/$USER/.pyenv/versions/3.10.6/bin/python"

Now you can run webui.sh

 ./webui.sh

