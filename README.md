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
   ---- permission deny의 경우, sudo vim /etc/ld.so.conf.d/cuda.conf 를 친후 직접 /usr/local/cuda/lib64 
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
sudo apt-get install build-essential libgtk2.0-dev libjpeg-dev libtiff4-dev libjasper-dev libopenexr-dev cmake python-dev python-numpy python-tk libtbb-dev libeigen3-dev yasm libfaac-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev libqt4-dev libqt4-opengl-dev sphinx-common libv4l-dev libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev default-jdk ant libvtk5-qt4-dev

 
sudo apt-get install build-essential cmake git


sudo apt-get install ffmpeg libopencv-dev libgtk-3-dev python-numpy python3-numpy libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libv4l-dev libtbb-dev qtbase5-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils unzip
 
-download opencv2.4.13

cd opencv-2.4.13

mkdir build
cd build

cmake -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_VTK=ON ..

make -j 8

sudo make install

sudo gedit /etc/ld.so.conf.d/opencv.conf
--- add   '/usr/local/lib'

sudo ldconfig

sudo gedit /etc/bash.bashrc

--- add   'PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig export PKG_CONFIG_PATH'



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
(python 2.7 & GPU supported)
export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp27-none-linux_x86_64.whl
sudo pip  install --upgrade TF_PYTHON_URL

