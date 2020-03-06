## valgrind  
sudo apt -y update  
sudo apt -y install valgrind cmake

## xfs  
This gives the kind of performance filesystem for PCIe installed nvme drives as in beanbox and roaster  
```bash
sudo apt -y install xfslibs-dev xfsdump xfsprogs
lsblk  # to know what you're working with... and if mounted as /nvme
sudo umount /nvme
sudo parted /dev/nvme0n1 rm 1
sudo parted /dev/nvme0n1 mklabel msdos
sudo parted --align optimal /dev/nvme0n1 mkpart primary xfs 0% 100%
lsblk # pake sure the partition /dev/nvme0n1p1 exists
sudo mkfs.xfs -L nvme -f /dev/nvme0n1p1
sudo vim /etc/fstab
sudo mkdir /nvme #if not existing already
sudo mount -av
sudo chown coffee /nvme
sudo chgrp data /nvme
sudo chmod g+rwx /nvme
```
now attempting a reboot to see if this is now available in gnome-disks app for formatting the nvme.  
This didn't work... for nvme memory to be performance, we need to use parted to create the filesystem.  
when editing /etc/fstab make sure the UUID bit is changed to LABEL=nvme


## h5py
```bash
pip3 install h5py
cd ${HOME}/Downloads
wget https://www.hdfgroup.org/downloads/hdf5/source-code/#
```


## Boost  
```bash
DIR=`pwd`
cd ${HOME}/Downloads  
BOOSTVER='1.72.0'
echo ${BOOSTVER}
BOOSTVERSTR=`echo "${BOOSTVER}" | sed -e 's/\./_/g'`
wget https://dl.bintray.com/boostorg/release/${BOOSTVER}/source/boost_${BOOSTVERSTR}.tar.bz2
cd /usr/local  
sudo tar -xjf ${HOME}/Downloads/boost_${BOOSTVERSTR}.tar.bz2  
rm ${HOME}/Downloads/boost_${BOOSTVERSTR}.tar.bz2  
sudo mkdir -p /opt/boost  
sudo ln -sf /usr/local/boost_${BOOSTVERSTR} /opt/boost/include  
```
*This next bit seems not to work on the home machines*  
```bash
sudo apt -y install libpython3.7-dev libboost-python-dev libboost-numpy-dev #libboost-numpy3-dev
sudo git clone https://github.com/ndarray/Boost.NumPy
cd Boost.NumPy
sudo mkdir build
cd build
sudo cmake ..
sudo make
sudo make install
```
# build and link  
```bash
cd /usr/local/boost_${BOOSTVERSTR}/  
sudo ./bootstrap.sh  
sudo ./b2 install
cd $DIR
```

## OpenMP  
Seems like this is already in modern c compilers, just using the -fopenmp flag in the compile  


## OpenMPI  
```bash
cd ${HOME}/Downloads   
wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.2.tar.gz   
cd /usr/local  
sudo tar -xzf ${HOME}/Downloads/openmpi-3.1.2.tar.gz  
cd openmpi-3.1.2  
sudo ./configure  
sudo make all install   
```


## FFTW3  
```bash
DIR=`pwd`
cd ${HOME}/Downloads  
FFTWVER=3.3.8
wget http://www.fftw.org/fftw-${FFTWVER}.tar.gz  
cd /usr/local  
sudo tar -xzf ${HOME}/Downloads/fftw-${FFTWVER}.tar.gz  
sudo wget http://www.fftw.org/fftw3.pdf  
cd fftw-${FFTWVER}
sudo ./configure --disable-fortran --enable-openmp --enable-threads --enable-single --with-gnu-ld  
```
	" only if building for TFLite or such acceleration in inferencing with FFTs --enable-single "  
	" also can't seem to get --enable-mpi to work"   
```bash
sudo make
sudo make all install   
sudo mkdir /opt/fftw3
sudo ln -sf /usr/local/fftw-${FFTWVER} /opt/fftw3/include
rm ${HOME}/Downloads/fftw-${FFTWVER}.tar.gz
cd $DIR 
```
**symlinking for sake of makefile ease... this is important!**  
```bash
cd /usr/lib/x86_64-linux-gnu
ls -a | grep fftw3
sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3f_omp.so.3 libfftw3f_omp.so  
sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3_omp.so.3 libfftw3_omp.so  
sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3.so.3 libfftw3.so  
sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3f.so.3 libfftw3f.so  
sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3f_threads.so.3 libfftw3f_threads.so  
sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3_threads.so.3 libfftw3_threads.so  
```

# or including fortran  
```bash
sudo ./configure --enable-openmp --enable-threads --enable-single --with-gnu-ld  
sudo make
sudo make install
```
# only succeed by ignoring MPI on both beanbox and roaster, 



## OpenCV  

```bash
DIR=`pwd`
sudo apt -y install build-essential
sudo apt -y install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt -y install python2-dev python3-numpy python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev
cd ${HOME}/Downloads
OPENCVVER=4.1.1
wget https://github.com/opencv/opencv/archive/${OPENCVVER}.zip
cd /usr/local
sudo unzip ${HOME}/Downloads/${OPENCVVER}.zip -d ./
cd opencv-${OPENCVVER}
sudo mkdir build
cd build
sudo cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local \
-DPYTHON3_EXECUTABLE=/usr/bin/python3 \
-DPYTHON_INCLUDE_DIR=/usr/include/python3.7 \
-DPYTHON_INCLUDE_DIR2=/usr/include/x86_64-linux-gnu/python3.7m \
-DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.7m.so \
-DPYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3.7/dist-packages/numpy/core/include/ ..
sudo make
sudo make install 
cd $DIR
sudo ln -sf /usr/local/opencv-4.1.1/build/lib /usr/local/lib/opencv
```
## OpenCV4 retry
When building with GStreamer add 
`pkg-config --cflags --libs gstreamer-1.0` to the gcc command
```bash
DIR=`pwd`
sudo apt -y install libopencv-dev build-essential cmake libdc1394-22 libdc1394-22-dev libjpeg-dev libpng-dev libtiff5-dev libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer-opencv1.0-0 libgstreamer1.0-0 libtbb-dev libqt4-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 
sudo apt -y install libgtk-3-dev libblas-dev liblapack-dev liblapack-doc checkinstall libeigen-stl-containers-dev libeigen3-dev default-jre default-jdk
sudo apt -y install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
========================================================== HERE HERE HERE ============================ building OpenCV4 but aim for 4.2.0 version Home machines
sudo apt -y install libcanberra-gtk-modu libcanberra-gtk3-module
cd /usr/local
sudo git clone https://github.com/opencv/opencv.git
sudo git clone https://github.com/opencv/opencv_contrib.git
sudo mkdir ./opencv/build
cd !$
sudo cmake -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local/opencv/build -DINSTALL_C_EXAMPLES=ON -DBUILD_EXAMPLES=ON -DOPENCV_EXTRA_MODULES_PATH=/usr/local/opencv_contrib/modules ../
sudo make -j4
sudo make install
```
It seems that we **may** need the file   
/usr/local/lib/pkgconfig/opencv4.pc  
with contents:
```bash
prefix=/usr/local
exec_prefix=${prefix}/opencv4
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: OpenCV
Description: OpenCV for computer vision
Version: 4.1.1
Libs: -L${libdir} -lopencv4
Libs.private: -lm
Cflags: -I${includedir}
```
**Also, it seems the libopencv4.1.1.so is not in or linked in /usr/local/opencv/build/lib/ so need to copy that from somewhere.**
	OK, his is nuts.  Now I'm removing the opencv_contrib to see if that helps  
adding the line **CVFLAGS= -ggdb `pkg-config --cflags --libs opencv`** to the makefile **now works**    

## Fall back OpenCV 3.2
sudo apt -y install cl-opencv-apps gstreamer1.0-opencv libcv-bridge-dev libgstreamer-opencv1.0-0 libopencv-apps-dev libopencv-calib3d-dev libopencv-contrib-dev libopencv-core-dev libopencv-dev libopencv-features2d-dev libopencv-flann-dev libopencv-highgui-dev libopencv-imgcodecs-dev libopencv-imgproc-dev libopencv-ml-dev libopencv-objdetect-dev libopencv-photo-dev libopencv-shape-dev libopencv-stitching-dev libopencv-superres-dev libopencv-ts-dev libopencv-video-dev libopencv-videoio-dev libopencv-videostab-dev libopencv-viz-dev libopencv3.2-java libopencv3.2-jni node-opencv opencv-data opencv-doc python-cv-bridge python-image-geometry python-opencv python-opencv-apps python-remotecv python3-image-geometry python3-opencv python3-opencv-apps
#OpenCV seems not to install on the coffee-T3500 machine

## Opencv-4.2.0

## Docker  
read this: https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce  
sudo apt remove docker docker-engine docker.io   
sudo apt -y update  
sudo apt -y install \  
    apt-transport-https \  
    ca-certificates \  
    curl \  
    software-properties-common  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
sudo apt-key fingerprint 0EBFCD88  
sudo add-apt-repository \  
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \  
   $(lsb_release -cs) \  
   stable"  
sudo apt -y update  
sudo apt -y install docker-ce  
sudo docker run hello-world  
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker
su -l $USER
docker run hello-world  

## Perl6 install  
sudo apt -y update  
sudo apt -y install build-essential libssl-dev  
cd ${HOME}/Downloads  
wget https://rakudo.org/latest/star/source   
cd /usr/local  
sudo tar xzf source   
sudo ln -s rakudo-star-* rakudo  
rm -f ${HOME}/source  
cd rakudo  
sudo perl Configure.pl --backend=moar --gen-moar  
sudo make
sudo make rakudo-test  
sudo make rakudo-spectest  
sudo make install  
echo "export PATH=$(pwd)/install/bin/:$(pwd)/install/share/perl6/site/bin:\$PATH" >> $HOME/.bashrc  
source $HIME/.bashrc  
cd ${HOME}  


## Video for Linux  
https://en.wikipedia.org/wiki/Video4Linux  

## Spatial  
https://spatial-lang.org/  
  
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list  
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823  
sudo apt update -Y  
sudo apt install -Y sbt  
sudo apt install -Y pkg-config libgmp3-dev libisl-dev  



## ParaView  
# NOT YET HERE...   
mkdir ${HOME}/computing  
cd !$  
wget https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.6&type=binary&os=Linux&downloadFile=ParaView-5.6.0-RC1-Qt5-MPI-Linux-64bit.tar.gz  
wget https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.6&type=data&os=Sources&downloadFile=ParaViewTestingData-v5.6.0-RC2.tar.xz  


## OpenCL  
sudo apt -y update  
sudo apt -y install ocl-icd-opencl-dev  
pip3 install pyopencl  ## this seems to have failed ##
  In file included from src/wrap_cl.hpp:60:0,
                   from src/wrap_constants.cpp:1:
  src/wrap_helpers.hpp:5:10: fatal error: pybind11/pybind11.h: No such file or directory
   #include <pybind11/pybind11.h>
            ^~~~~~~~~~~~~~~~~~~~~
  compilation terminated.
  error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
pip install pyopencl  ## also seems to fail the same way ##


## HDF5  
http://docs.h5py.org/en/stable/build.html
https://www.hdfgroup.org/downloads/hdf5/source-code/
git clone https://github.com/live-clones/hdf5.git

## Used for Bazel   
# maybe going to stick with make until absolutely necessary
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python  
Installing using binary installer  
The binary installers are on Bazel's GitHub releases page.  

The installer contains the Bazel binary1. Some additional libraries must also be installed for Bazel to work.  

Step 1: Install required packages  
First, install the prerequisites: pkg-config, zip, g++, zlib1g-dev, unzip, and python.  

sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python  
Step 2: Download Bazel  
Next, download the Bazel binary installer named bazel-<version>-installer-linux-x86_64.sh from the Bazel releases page on GitHub.  

Step 3: Run the installer  
Run the Bazel installer as follows:  

chmod +x bazel-<version>-installer-linux-x86_64.sh  
./bazel-<version>-installer-linux-x86_64.sh --user  
The --user flag installs Bazel to the $HOME/bin directory on your system and sets the .bazelrc path to $HOME/.bazelrc. Use the --help command to see additional installation options.  

Step 4: Set up your environment  
If you ran the Bazel installer with the --user flag as above, the Bazel executable is installed in your $HOME/bin directory. It's a good idea to add this directory to your default paths, as follows:  

export PATH="$PATH:$HOME/bin"  
You can also add this command to your ~/.bashrc file.  


