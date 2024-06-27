Check the c++ version installed 
```
g++ --version
```
If output looks like 
```
g++ (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
```
GCC 4.8 and above support C++11, so any recent version, such as the one mentioned above, will work.  

# Install Pangolin
Download pangolin  
https://github.com/stevenlovegrove/Pangolin  

```
git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
./scripts/install_prerequisites.sh --dry-run recommended
cmake -B build
cmake --build build
cmake --build build -t pypangolin_pip_install
```
Successfully installed numpy-2.0.0 pillow-10.3.0 pybind11-2.12.0 pyopengl-3.1.7 pyopengl-accelerate-3.1.7 pypangolin-0.9.1


While executing above command i encountered an error 
E: Unable to locate package catch2

I resolved it by building catch2 from git 
```
git clone https://github.com/catchorg/Catch2.git
cd Catch2
cmake -B build -S . -DBUILD_TESTING=OFF
sudo cmake --build build/ --target install
```
After Catch2 installation i removed all mentions of catch2 from install_prereqisites.sh and ran the install_prerequisites.sh again

# Install opencv (code tested with version 4.4.0)
```

sudo apt-get install -y  libjpeg-dev libpng-dev libtiff-dev libgtk2.0-dev libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libeigen3-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev sphinx-common libtbb-dev yasm libfaac-dev libopencore-amrnb-dev libopencore-amrwb-dev libopenexr-dev libgstreamer-plugins-base1.0-dev libavutil-dev libavfilter-dev libavresample-dev
6
```
4.4.0.40 caused failure while building wheel, numpy 2.0.0 causing error while loading cv2

# Install Eigen
Eigen is required by g2o
Download eigen zip file https://eigen.tuxfamily.org/index.php?title=Main_Page and extract
```
cd eigen
mkdir build_dir
cd build_dir
cmake ../
make install
```

# Install DBoW2

```
sudo apt-get install libopencv-dev

git clone https://github.com/dorian3d/DBoW2.git
cd DBoW2

mkdir build
cd build

cmake ..

make

sudo make install
```

Check if the library files are installed
```
ls /usr/local/lib/libDBoW2.so
```
Check if the header files are installed
```
ls /usr/local/include/DBoW2/
```

# Install g2o
Install dependencies
```
sudo apt install libeigen3-dev libspdlog-dev libsuitesparse-dev qtdeclarative5-dev qt5-qmake libqglviewer-dev-qt5
```

Install g2o
```
git clone https://github.com/RainerKuemmerle/g2o.git
cd g2o
mkdir build
cd build
cmake ../
make
```

# Install ROS

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt install curl # if you haven't already installed curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

sudo apt update

sudo apt install ros-noetic-desktop-full
```
env setup

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Install dependencies
```
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```
Initialize rosdep
```
sudo rosdep init
rosdep update
```

# Install intel realsesne camera setup

```
sudo apt-get install libssl-dev libusb-1.0-0-dev libudev-dev pkg-config libgtk-3-dev v4l-utils
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev at
git clone https://github.com/IntelRealSense/librealsense.git
```
