Oto ładnie sformatowany dokument z instrukcjami, gotowy do użycia w pliku README.md lub podobnym dokumencie:

---

# Stable Diffusion WebUI with ROCm on Arch Linux/Manjaro (AMD GPU)

## How to Install ROCm on Arch Linux or Arch Linux Variants

### 1. System Update

#### For Arch Linux:
```bash
sudo pacman -Syyu
```

#### For Manjaro:
```bash
sudo pacman-mirrors -f && sudo pacman -Syyu
```
**OR**
```bash
sudo pamac upgrade -a
```

### 2. Install Yay
```bash
sudo pacman -S yay
```

### 3. Install ROCm and Dependencies
```bash
yay -S git python-virtualenv wget python-pip dpkg make rocm-hip-sdk rocm-opencl-sdk gperftools bc pyenv
```

### 4. Add User to the Render Group
Remove your user from the `render` and `video` groups:
```bash
sudo gpasswd -d $USER render
sudo gpasswd -d $USER video
```

### 5. Edit Profile or Environment

Edit `~/.profile` with your preferred editor:
```bash
sudo nano ~/.profile
```
**OR**

Edit `/etc/environment`:
```bash
sudo nano /etc/environment
```

**Note:**  
- `~/.profile` affects the current user's profile.
- `/etc/environment` applies system-wide on bootup. If using `/etc/environment`, remove the `export` keyword.

Add the following line at the bottom of the file:
```bash
export HSA_OVERRIDE_GFX_VERSION=10.3.0
```
*(Change the version number to `11.0.0` for 7000 series GPUs)*

### 6. Reboot the System
```bash
reboot
```

### 7. Install Python 3.10.6
```bash
pyenv install 3.10.6
```

This command installs Python 3.10.6 in your home directory. Navigate to this directory:
```bash
cd /home/$USER/.pyenv/versions/3.10.6/bin/
```

Create and activate a virtual environment:
```bash
python -m venv venv
. venv/bin/activate
```

The prompt should change to `(venv)`.

### 8. Install Wheel
Separate the following commands to ensure proper installation order:
```bash
./pip install wheel
```

### 9. Install PyTorch with ROCm Support
```bash
./pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm5.7
```

### 10. Verify PyTorch Installation
Enter the Python console:
```bash
./python3
```

Then enter the following code to verify GPU support:
```python
import torch
torch.cuda.is_available()
```

If it returns `True`, your GPU is working correctly. Exit the Python console:
```python
exit()
```

### 11. Clone Stable Diffusion WebUI Repository

Clone the repository to your home folder or another directory:
```bash
cd
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
cd stable-diffusion-webui
nano webui-user.sh
```

### 12. Configure Python Path in `webui-user.sh`

Change the Python command line in `webui-user.sh` to:
```bash
python_cmd="/home/$USER/.pyenv/versions/3.10.6/bin/python"
```

### 13. Run WebUI
```bash
./webui.sh
```

---

Ten dokument jest teraz gotowy do użycia w Twoim projekcie na GitHubie lub innym miejscu.
