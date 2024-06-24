Check the c++ version installed 
```
g++ --version
```
If output looks like 
```
g++ (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
```
GCC 4.8 and above support C++11, so any recent version, such as the one mentioned above, will work.  

Download pangolin  
https://github.com/stevenlovegrove/Pangolin  
# Clone Pangolin along with it's submodules
```
git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
./scripts/install_prerequisites.sh --dry-run recommended
```

While executing above command i error 
E: Unable to locate package catch2

I resolved it by building catch2 from git 
```
git clone https://github.com/catchorg/Catch2.git
cd Catch2
cmake -B build -S . -DBUILD_TESTING=OFF
sudo cmake --build build/ --target install
```
After Catch2 installation i removed all mentions of catch2 from install_prereqisites.sh and ran the install_prerequisites.sh again