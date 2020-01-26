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
sudo apt -y install python3-venv python3-pip python3-setuptools  
sudo apt -y install python-pip python-setuptools    
pip3 install numpy scipy sklearn   
# maybe wait to do tensorflow2.0 as a source build  
pip3 install tensorflow   
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

