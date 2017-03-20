# Ubuntu14.04-Cuda8.0-cudnn5.1-opencv2.4.13-tensorflow1.0-gym


0. INSTALL Ubuntu 14.04 LTS
(Ubuntu 14.04 LTS Installation w/ window 10 multi-booting)

Impelementing UEFI version for Ubuntu USB

[[[ Grub-Repair (optional) - try Ubuntu w/o installing Ubuntu ]]]

sudo add-apt-repository ppa:yannubuntu/boot-repair

sudo apt-get update

sudo apt-get install -y boot-repair

boot-repair

[[[ Installing Ubuntu 14.04-LTS ]]]

-Three partitions are needed: swap, /, /home

-swap: about 2 times as memory (Logical, Beginning of this space)

-/: most of volume (Logical, Beginning of this space, ext4)(It's kind of C drive in Window)

-/home: rest of volume (Logical, Beginning of this space, ext4)

-In my case, from 200GB, I assigned 16GB for swap, 120GB for /, and the rest for /home.

** Device for boot lader : Window boot Manager ( not the disk )

[[[ Synatics ]]]

sudo apt-get update

sudo apt-get install wget gdebi

wget http://http.us.debian.org/debian/pool/main/s/synaptiks/kde-config-touchpad_0.8.1-2_all.deb

sudo gdebi kde-config-touchpad_0.8.1-2_all.deb

sudo apt-mark hold kde-config-touchpad

synaptiks

1. INSTALL CUDA ( cuda 8.0 )
-download cuda (run file)
- Ctrl + Alt + F1~6 (for tty1~6); then, login
- sudo service lightdm stop
- cd path/to/cuda/file
- chmod +x name_of_cuda_installer
- sudo ./name_of_cuda_installer
- install cuda WITHOUT OPENGL (Note that with opengl may cause login loop)
- sudo service lightdm start
- 로그인이 안되는 경우, nvidia driver문제 (without opengl인지 확인할것)
- login infinite loop시] sudo apt-get remove --purge nvidia-* --> sudo apt-get install ubuntu-desktop
- 환경변수 


cd ~
sudo vim .bashrc
<< Add followings >>
    # CUDA
    export CUDA_ROOT=/usr/local/cuda/
    export CUDA_HOME=/usr/local/cuda/
    export PATH=$PATH:/usr/local/cuda/bin/
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
    export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda/lib64
    export CPATH=$CPATH:/usr/local/cuda/include
 sudo echo "/usr/local/cuda/lib64" > /etc/ld.so.conf.d/cuda.conf
   ---- permission deny의 경우, sudo vim /etc/ld.so.conf.d/cuda.conf 를 친후 직접 /usr/local/cuda/lib64          // must do it!!!
 sudo ldconfig


2. cudnn (cudnn 5.1)
cd path/to/cudnn/file (~/cuda)
sudo cp include/ /usr/local/cuda-*/include/
   -----  or) sudo cp -r include/ /usr/local/cuda-.*/include/
sudo cp lib64/ /usr/local/cuda-.*/lib64/
   -----  or) sudo cp -r lib64/ /usr/local/cuda-.*/lib64/

*** PLEASE Make sure that the file is really copied to /usr/ ~~~~~ 
if) the above is not working, follow the below....:
sudo cp cuda/include/cudnn.h /usr/local/cuda/include 
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
(---optional)
      sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
      
3. NVIDIA driver (optional, if needed) 
-download driver (run file)
-Ctrl + Alt + F1~6
sudo service lightdm stop
cd path/to/driver/file
chmod +x name_of_driver
sudo ./name_of_driver --no-opengl-files
sudo service lightdm start


4. opencv 2.4.13 ---gpu supported!!!!  
http://www.linuxidc.com/Linux/2016-12/138865.htm

sudo apt-get update
sudo apt-get install -y --no-install-recommends build-essential cmake libavcodec-dev libavformat-dev libgtk2.0-dev libgtkglext1 libgtkglext1-dev libjpeg-dev  libpng-dev libswscale-dev libtbb2 libtbb-dev libtiff-dev pkg-config unzip wget

cd opecvn-2.

mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_CUDA=ON -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON -D WITH_CUBLAS=1 -D WITH_NVCUVID=on -D CUDA_GENERATION=Auto ..


make -j 8

sudo make install

sudo gedit /etc/ld.so.conf.d/opencv.conf
--- add   '/usr/local/lib'

sudo ldconfig

sudo gedit /etc/bash.bashrc

--- add   'PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig export PKG_CONFIG_PATH'

sudo apt-get install python-opencv

then, test import cv2

5. gym
git clone https://github.com/openai/gym
cd gym
sudo pip install --upgrade pip   
pip install -e .[all]



6. arcade-learning-environment
-download from https://github.com/mgbellemare/Arcade-Learning-Environment
sudo apt-get install libsdl1.2-dev libsdl-gfx1.2-dev libsdl-image1.2-dev cmake
mkdir build && cd build
cmake -DUSE_SDL=ON -DUSE_RLGLUE=OFF -DBUILD_EXAMPLES=ON ..
make -j 4
cd ..
pip install .
   --- or) sudo -H pip install .


7. tensorflow 1.0
http://tflearn.org/installation/

(python 2.7 & GPU supported)
export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp27-none-linux_x86_64.whl
sudo pip  install --upgrade TF_BINARY_URL

# Ubuntu/Linux 64-bit, CPU only, Python 2.7
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 2.7
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp27-none-linux_x86_64.whl

# Mac OS X, CPU only, Python 2.7:
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.0-py2-none-any.whl

# Mac OS X, GPU enabled, Python 2.7:
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/gpu/tensorflow_gpu-1.0.0-py2-none-any.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.3
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp33-cp33m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.3
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp33-cp33m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.4
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.4
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.5
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.5
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.6
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp36-cp36m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.6
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp36-cp36m-linux_x86_64.whl

# Mac OS X, CPU only, Python 3.4 or 3.5:
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.0-py3-none-any.whl

# Mac OS X, GPU enabled, Python 3.4 or 3.5:
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/gpu/tensorflow_gpu-1.0.0-py3-none-any.whl

