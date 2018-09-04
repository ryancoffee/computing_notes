##Nominal base
sudo apt update
sudo apt install vim bc grpn gnuplot

#Nice extras for GUI 
sudo apt install gimp g3data

##Used for Bazel
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


coffee@beanbox:~$ ip route | grep default
default via 192.168.1.254 dev eno1 proto dhcp metric 100 

This is for building and getting ssh running

coffee@beanbox:~$ sudo apt update
[sudo] password for coffee: 
coffee@beanbox:~$ sudo apt update
[sudo] password for coffee: 
coffee@beanbox:~$ sudo apt install net-tools openssh-server
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.factory-defaults
sudo systemctl restart ssh

