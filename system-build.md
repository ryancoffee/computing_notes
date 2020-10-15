## Nominal base  
sudo apt -y update  
sudo apt -y upgrade
sudo apt -y install git vim bc gnuplot   
git config --global user.email "coffeer76@gmail.com"  
git config --global user.name "Ryan Coffee"  


## This is for building and getting ssh running  

coffee@beanbox:~$ ip route | grep default  
default via 192.168.1.254 dev eno1 proto dhcp metric 100   

sudo apt -y update  
sudo apt -y install net-tools openssh-server  
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.factory-defaults  
sudo systemctl restart ssh  

## Python and pip  
sudo apt -y install python3-venv python3-dev python3-pip python3-setuptools  
sudo apt -y install python-pip python-setuptools    
pip install --upgrade pip
pip3 install numpy scipy sklearn tensorflow pydot graphviz
# maybe wait to do tensorflow2.0 as a source build  
pip install numpy scipy	tensorflow    

## Nice extras for GUI   
sudo apt -y install gimp grpn g3data dia geeqie pandoc

## Chrome
sudo apt -y install libxss1 libappindicator1 libindicator7  
cd $HOME/Downloads  
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb  
sudo dpkg -i google-chrome*.deb  
rm google-chrome*.deb  
cd -


## 2020 update latex 
cd ~/Downloads  
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
wget http://mirrors.ctan.org/macros/latex/contrib/revtex.zip
sudo unzip revtex.zip -d /usr/local/texlive/
cd install-tl-*/
sudo ./install-tl  
cd ../  
rm -rf install-tl-*  


## latex  
cd ~/Downloads  
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz  
tar -xzf install-tl-unx.tar.gz  
cd install-tl-*/
sudo ./install-tl  
cd ../  
rm -rf install-tl-*  

## pavoni/roaster/beanbox specifics
for nvme to use xfs for performance  
```bash
sudo apt -y update
sudo apt -y install xfslibs-dev xfsdump xfsprogs
sudo addgroup data 
sudo usermod -a -G data coffee
sudo usermod -a -G data root
sudo chgrp data /data
sudo chmod g+w /data
sudo chown coffee /nvme
sudo chgrp data /nvme
sudo chmod g+rwx /nvme
```

## For tethering a digital camera  
```bash
sudo apt -y install gphoto2 gtkam entangle
```

## building with NVMe available to the installer  
https://www.dell.com/support/article/en-us/sln299303/loading-ubuntu-on-systems-using-pcie-m2-drives?lang=en

## headless for nvme addition
updating /etc/fstab with sudo on roaster... running it headless (no GPU/videocard at all)   
```
#LABEL=nvme                               /nvme           xfs     defaults        0       2
UUID=b61f6e8f-9d86-4432-92c2-2574621d79cf /nvme           xfs     defaults        0       2
UUID=832acdf0-6491-499f-8bbe-bf2dc946fcad /nvme1TB        xfs     defaults        0       2
/swapfile                                 none            swap    sw              0       0
```
for instance, this is how we find the UUID for the drives... and indeed using the UUID is a more robust way to keep track of the devices.  
```
coffee@roaster:/$ sudo blkid /dev/nvme0n1p1 
/dev/nvme0n1p1: LABEL="nvme1TB" UUID="832acdf0-6491-499f-8bbe-bf2dc946fcad" TYPE="xfs" PARTUUID="b96ae363-01"
coffee@roaster:/$ sudo blkid /dev/nvme1n1p1 
/dev/nvme1n1p1: LABEL="nvme" UUID="b61f6e8f-9d86-4432-92c2-2574621d79cf" TYPE="xfs" PARTUUID="f488b8f7-01"
coffee@roaster:/$ vim /etc/fstab 
```

## headless startup disk creator  
The GUI for startup disk creator is...  
```
currentdir=`pwd`
cd $HOME/Downloads
wget http://releases.ubuntu.com/20.04/ubuntu-20.04-beta-desktop-amd64.iso  
usb-creator-gtk
```
This doesn't seem to work.  
