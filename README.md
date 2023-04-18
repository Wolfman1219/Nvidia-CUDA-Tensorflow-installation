# Nvidia-CUDA-Tensorflow-installation
## Install nvidia driver
### For debian based:
You can check whether your computer has an NVIDIA GPU installed or not.
Open the terminal and type
```shell
lspci | egrep 'VGA|NVIDIA'
```
NVIDIA drivers are available in the official contrib and non-free package repositories of Debian 11.
To enable the contrib package repository, run the following command:
```shell
sudo apt-add-repository contrib
sudo apt-add-repository non-free
```

To update the APT package database, run the following command:
```shell
sudo apt update
```

Install nvidia-driver:
```shell
sudo apt install nvidia-driver
```
```shell 
sudo reboot
```

### For arch based:

For checking has GPU or not:
```shell
lspci | egrep 'VGA|NVIDIA'
```
And run this commanlines:
```shell
sudo pacman -Syu
```
Install git and clone this:
```shell
sudo pacman -S git
git clone https://gitlab.com/XavierEduardo99/nvidia-drivers-arch-linux-installer
```
Enter this directory:
```shell
cd nvidia-drivers-arch-linux-installer
```
If your kernel linux:
```shell
sudo sh install-nvidia.sh
```
If your kernel is linux-lts:
```shell
sudo sh install-nvidia.sh --lts
```
and reboot system

## Install conda
Just run this commands in your terminal:
```shell
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

Follow the instruction from offical site.

## Install cuda and cudnn for tensorflow
Create a virtual environment for your TensorFlow project and activate it.
```shell
conda create --name tf-gpu python=3.9
conda activate tf-gpu
```
Install required packages and libraries:
```shell
conda install -c conda-forge cudatoolkit=11.8.0
pip install nvidia-cudnn-cu11==8.6.0.163
```
Configure the system paths.The system paths will be automatically configured when you activate this conda environment.
```shell
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```
LIFEHACK! If you want use cuda and cudnn without activating conda try this steps. WARNING!!If you can't fix after system crash, i don't recommend do this.
If you believe in yourself so much, then let's start.
Activate cuda installed environment:
```shell
conda activate tf-gpu
```
View cuda path:
```shell
python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"
```
Copy this path and run command.Replace path/to/cuda with copied path. WARNING copy directory path not file path!!!:
```shell
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/lib/:path/to/cuda/lib'
```
Well done! You are set cuda path.

If when open new terminal tensorflow don't see GPU. Add ```export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/lib/:path/to/cuda/lib'``` to ```.bashrc``` file.
## Install tensorflow
```shell
conda activate tf-gpu
pip install --upgrade pip
pip install tensorflow==2.12.*
```

If all this steps doing well following command print GPU list:
```shell
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```
# Good luck
