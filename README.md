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
pip install numpy==1.19.5
pip install opencv-python==4.4.0.46
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
