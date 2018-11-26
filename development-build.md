## Boost  
cd ${HOME}/Downloads  
wget https://dl.bintray.com/boostorg/release/1.68.0/source/boost_1_68_0.tar.bz2  
cd /usr/local  
sudo tar -xjf ${HOME}/Downloads/boost_1_68_0.tar.bz2  
rm ${HOME}/Downloads/boost_1_68_0.tar.bz2  
sudo mkdir -p /opt/boost  
sudo ln -sf /usr/local/boost_1_68_0 /opt/boost/include  

# build and link  
cd /usr/local/boost_1_68_0/  
sudo ./bootstrap.sh  
sudo ./b2 install


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
sudo ./configure --disable-fortran --enable-openmp --enable-threads --enable-single --with-gnu-ld  
	" only if building for TFLite or such acceleration in inferencing with FFTs --enable-single "  
	" also can't seem to get --enable-mpi to work"   
sudo mkdir /opt/fftw3
sudo ln -s /usr/local/fftw-3.3.8 /opt/fftw3/include

# symlinking for sake of makefile ease
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ ls -a | grep fftw3
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3f_omp.so.3 libfftw3f_omp.so  
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3_omp.so.3 libfftw3_omp.so  
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3.so.3 libfftw3.so  
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3f.so.3 libfftw3f.so  
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3f_threads.so.3 libfftw3f_threads.so  
coffee@pavoni:/usr/lib/x86_64-linux-gnu$ sudo ln -s /usr/lib/x86_64-linux-gnu/libfftw3_threads.so.3 libfftw3_threads.so  


# or including fortran  
sudo ./configure --enable-openmp --enable-threads --enable-single --with-gnu-ld  
sudo make
sudo make install
# only succeed by ignoring MPI on both beanbox and roaster, 

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





## ParaView  
# NOT YET HERE...   
mkdir ${HOME}/computing  
cd !$  
wget https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.6&type=binary&os=Linux&downloadFile=ParaView-5.6.0-RC1-Qt5-MPI-Linux-64bit.tar.gz  
wget https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.6&type=data&os=Sources&downloadFile=ParaViewTestingData-v5.6.0-RC2.tar.xz  


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


