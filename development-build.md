## Boost  
cd ${HOME}/Downloads  
wget https://dl.bintray.com/boostorg/release/1.68.0/source/boost_1_68_0.tar.bz2  
cd /usr/local  
sudo tar -xjf ${HOME}/Downloads/boost_1_68_0.tar.bz2  
rm ${HOME}/Downloads/boost_1_68_0.tar.bz2  
sudo mkdir -p /opt/boost  
sudo ln -sf /usr/local/boost_1_68_0 /opt/boost/include  

## OpenMP  
Seems like this is already in modern c compilers, just using the -fopenmp flag in the compile  


## OpenMPI  
cd ${HOME}/Downloads   
wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.2.tar.gz   
cd /usr/local  
sudo tar -xzf ${HOME}/Downloads/openmpi-3.1.2.tar.gz  
cd openmpi-3.1.2  
sudo ./configure  
sudo make all install   
# made it here so far  


## FFTW3  
cd ${HOME}/Downloads  
wget http://www.fftw.org/fftw-3.3.8.tar.gz  
cd /usr/local  
sudo tar -xzf ${HOME}/Downloads/fftw-3.3.8.tar.gz  
sudo wget http://www.fftw.org/fftw3.pdf  
cd fftw-3.3.8  
sudo ./configure --disable-fortran --enable-openmp --enable-threads --with-gnu-ld  
	" only if building for TFLite or such acceleration in inferencing with FFTs --enable-single "  
	" also can't seem to get --enable-mpi to work"   
sudo make
sudo make install
# only succeed by ignoring MPI on both beanbox and roaster, 

## ParaView  
NOT YET HERE...  
mkdir ${HOME}/computing  
cd !$  
wget https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.6&type=binary&os=Linux&downloadFile=ParaView-5.6.0-RC1-Qt5-MPI-Linux-64bit.tar.gz  
wget https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.6&type=data&os=Sources&downloadFile=ParaViewTestingData-v5.6.0-RC2.tar.xz  


## Perl6 install  
wget https://rakudo.perl6.org/downloads/star/rakudo-star-2018.06.tar.gz  
tar xfz rakudo-star-2018.06.tar.gz  
cd rakudo-star-2018.06  
perl Configure.pl --gen-moar --make-install --prefix ~/rakudo  
ln -s ~/rakudo/bin/perl6 /usr/bin/perl6  


## Used for Bazel   
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


