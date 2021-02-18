## valgrind  
```bash
sudo apt -y update  
sudo apt -y install valgrind cmake lm-sensors hddtemp
```
then test sensors in a terminal with 
```bash
watch -n 2 sensors


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
sudo blkid # to get the UUID for hte drive, use this in /etc/fstab rather than label=nvme
sudo vim /etc/fstab
sudo mkdir /nvme #if not existing already
sudo mount -av
sudo chown -R coffee:data /nvme
sudo chmod 755 /nvme
```
now attempting a reboot to see if this is now available in gnome-disks app for formatting the nvme.  
This didn't work... for nvme memory to be performance, we need to use parted to create the filesystem.  
when editing /etc/fstab make sure the UUID bit is changed to LABEL=nvme
CORRECTION... going back to UUID since that travels with the hardware


## h5py  
```bash
pip3 install h5py
cd ${HOME}/Downloads
wget https://www.hdfgroup.org/downloads/hdf5/source-code/#
```

##LeCroy Parser for .trc files  
```bash
pip3 install lecroyparser
```
This you can use to see the dimensions of the data and from that infer the header.  Then you can just skip the header and do a binary read of the file.  
This would likely save memory since the binary read has only one byte per sample, but the lecroyparser scales the data by voltage and so has to use 4 bytes for the 32 bit floats.  


## Boost  
```bash
DIR=`pwd`
cd ${HOME}/Downloads  
BOOSTVER='1.73.0'
echo ${BOOSTVER}
BOOSTVERSTR=`echo "${BOOSTVER}" | sed -e 's/\./_/g'`
wget https://dl.bintray.com/boostorg/release/${BOOSTVER}/source/boost_${BOOSTVERSTR}.tar.bz2
cd /usr/local  
sudo tar -xjf ${HOME}/Downloads/boost_${BOOSTVERSTR}.tar.bz2  
rm ${HOME}/Downloads/boost_${BOOSTVERSTR}.tar.bz2  
#sudo mkdir -p /opt/boost  
#sudo ln -sf /usr/local/boost_${BOOSTVERSTR} /opt/boost/include  
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
sudo ./b2 headers
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
cd /usr/local/fftw-${FFTWVER}
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

## HDF5  
https://www.hdfgroup.org/downloads/hdf5/
http://docs.h5py.org/en/stable/build.html
https://www.hdfgroup.org/downloads/hdf5/source-code/
https://www.hdfgroup.org/downloads/hdf5

obtain the tarball from the website...
unzip it to /usr/local

```bash
cd /usr/local/
sudo git clone https://bitbucket.hdfgroup.org/scm/hdffv/hdf5.git
cd hdf5
./configure --prefix=/usr/local/hdf5 --enable-cxx
```

## HDF5 old
```bash
cd ${HOME}/Downloads
wget https://www.hdfgroup.org/downloads/hdf5/source-code/#
wget http://prdownloads.sourceforge.net/libpng/zlib-1.2.11.tar.gz
cd /usr/local
tar -xzf ${HOME}/Downloads/zlib-1.2.11.tar.gz
sudo make ./zlib-1.2.11/build
cd ./zlib-1.2.11/build
sudo ../configure
sudo make
sudo make install
cd /usr/local
sudo git clone https://github.com/live-clones/hdf5.git
cd hdf5
sudo mkdir build
cd build
sudo ../configure --prefix=/usr/local --with-gnu-ld --enable-cxx
sudo make
sudo make install
```

## qucs-spice circuit simulator (quite universal circuit simulator - spice)  
snap install qucs-spice

## non snap way to install
```bash
cd ${HOME}/Downloads
wget -c http://download.opensuse.org/repositories/home:/ra3xdh/Debian_9.0/Release.key
sudo vim /etc/apt/sources.list
```
add `deb http://download.opensuse.org/repositories/home:/ra3xdh/Debian_9.0/ ./` to the file /etc/apt/sources.list
```bash
sudo apt-key add Release.key
sudo apt-get update
sudo apt-get install qucs-s
```


## OpenCV  
Updated to python3.8, opencv-4.2.0 ... I think...  
*OK this seems to work*  
  
Nevermind... also noticed we need to find lapack and openblas   
Trying this first and then rebuilding   
Still Trying   
First install Eigen (this is a headers only library, but then point openCV to it)  
```bash  
cd ${HOME}/Downloads
wget https://gitlab.com/libeigen/eigen/-/archive/3.3.7/eigen-3.3.7.tar.gz
cd /usr/local/include
sudo tar -xzf ${HOME}/Downloads/eigen-3.3.7.tar.gz
sudo ln -sf /usr/local/include/eigen-3.3.7/Eigen /usr/local/include/Eigen
```  

```bash  
sudo apt -y install build-essential
sudo apt -y install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt -y install python2-dev python3-numpy python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev
sudo apt install -y libblas-dev libblas64-dev libopenblas-base libopenblas-dev libopenblas-openmp-dev libopenblas-pthread-dev libopenblas-serial-dev mlpack-bin mlpack-doc python3-cvxopt python3-mlpack liblapack64-3 liblapack64-dev
sudo apt install -y libjs-jquery-ui-docs tcl8.6 tk8.6
```  

#HERE HERE HERE HERE#
*try removing gstreamer1.0-opencv and purge the system of opencv that might be conflicting with this opencv4*
sudo apt install -y gstreamer1.0-qt5 gstreamer1.0-plugins-base gstreamer1.0-opencv

```bash  
sudo apt install -y python-attr-doc python-cvxopt-doc python-cycler-doc python3-genshi python3-lxml-dbg python-lxml-doc dvipng ffmpeg inkscape ipython3 python-matplotlib-doc python3-cairocffi python3-nose
sudo apt install -y python3-pyqt5 python3-sip python3-tornado texlive-extra-utils texlive-latex-extra ttf-staypuft python-pandas-doc python3-statsmodels subversion python-pyparsing-doc python-scipy-doc python3-netcdf4 python-tables-doc vitables tix python3-tk-dbg
sudo apt install -y libcusolver10 libcusolvermg10 libmkl-full-dev libmkl-scalapack-ilp64 libmkl-scalapack-lp64
sudo apt -y install qtbase5-dev qtdeclarative5-dev
sudo apt -y install opencl-headers ocl-icd-libopencl1
cd ${HOME}/Downloads
cvVersion=4.3.0
wget https://github.com/opencv/opencv/archive/${cvVersion}.tar.gz -O opencv.tar.gz
wget https://github.com/opencv/opencv/opencv_contrib/archive/${cvVersion}.tar.gz -O opencv_contrib.tar.gz
https://github.com/opencv/opencv_contrib.git
cd /usr/local
sudo tar -xzf ${HOME}/Downloads/opencv.tar.gz
sudo tar -xzf ${HOME}/Downloads/opencv_contrib.tar.gz
sudo mkdir -p /usr/local/opencv-${cvVersion}/build
cd /usr/local/opencv-${cvVersion}/build
```  
*Old method... keeps failing on roaster*   
... trying now to turn off Eigen and the examples  
... this seems to have done it.  
```bash
sudo cmake -DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/usr/local \
-DOPENCV_EXTRA_MODULES_PATH=/usr/local/opencv_contrib/modules \
-DINSTALL_C_EXAMPLES=OFF \
-DBUILD_EXAMPLES=OFF \
-DWITH_EIGEN=OFF \
-DWITH_LAPACK=ON \ 
-DWITH_OPENGL=ON \
-DWITH_QT=ON \
-DPYTHON3_EXECUTABLE=/usr/bin/python3 \
-DPYTHON_INCLUDE_DIR=/usr/include/python3.8 \
-DPYTHON_INCLUDE_DIR2=/usr/include/x86_64-linux-gnu/python3.8 \
-DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.8m.so \
-DPYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3.8/dist-packages/numpy/core/include/ \
-DBUILD_opencv_rgbd=OFF \
..
sudo ln -sf /usr/local/include/eigen-3.3.7/ /usr/local/include/eigen
sudo ln -sf /usr/local/include/eigen-3.3.7/unsupported /usr/local/include/unsupported
sudo make -j4 
sudo make install 
cd $DIR
```   

sudo ln -df /usr/local/lib/libopencv_core.so.4.2.0 ../../build/lib/libopencv_core.so.4.2
```sudo ln -sf /usr/local/opencv-${cvVersion}/build/lib /usr/local/lib/opencv4```

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



OpenCV4 failed retry   
*likely not necessary below here*  
When building with GStreamer add 
`pkg-config --cflags --libs gstreamer-1.0` to the gcc command
```bash
DIR=`pwd`
sudo apt -y install libopencv-dev build-essential cmake libdc1394-22 libdc1394-22-dev libjpeg-dev libpng-dev libtiff5-dev libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer-opencv1.0-0 libgstreamer1.0-0 libtbb-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 
sudo apt -y install qtbase5-dev
sudo apt -y install libgtk-3-dev libblas-dev liblapack-dev liblapack-doc checkinstall libeigen-stl-containers-dev libeigen3-dev default-jre default-jdk
sudo apt -y install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
sudo apt -y install libcanberra-gtk-module libcanberra-gtk3-module
========================================================== HERE HERE HERE ============================ building OpenCV4 but aim for 4.2.0 version Home machines
cd /usr/local
sudo git clone https://github.com/opencv/opencv.git
sudo git clone https://github.com/opencv/opencv_contrib.git
sudo mkdir ./opencv/build
cd ./opencv/build 
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
```bash
cd ${HOME}/Downloads
wget http://prdownloads.sourceforge.net/libpng/zlib-1.2.11.tar.gz
cd /usr/local
tar -xzf ${HOME}/Downloads/zlib-1.2.11.tar.gz
sudo make ./zlib-1.2.11/build
cd ./zlib-1.2.11/build
sudo ../configure
sudo make
sudo make install
cd /usr/local
sudo git clone https://github.com/live-clones/hdf5.git
cd hdf5
sudo mkdir build
cd build
sudo ../configure --prefix=/usr/local --with-gnu-ld --enable-cxx
sudo make
sudo make install
pip3 install h5py
```

## PyPENELOPE  
```bash
pip3 install matplotlib pyparsing wxpython
cd ${HOME}/Downloads
wget http://sourceforge.net/projects/pypenelope/files/0.2.10/pypenelope-0.2.10-binaries.tar.gz
wget http://sourceforge.net/projects/pypenelope/files/0.2.10/pypenelope-0.2.10.tar.gz
cd /usr/local
mkdir pypenelope
cd !$
mv ${HOME}/Downloads/pypenelope*.tar.gz ./

```
wxpyton fails  

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


