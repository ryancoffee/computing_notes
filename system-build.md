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
pip3 install numpy scipy tensorflow  
pip install numpy scipy	tensorflow    

## Nice extras for GUI   
sudo apt -y install gimp grpn g3data dia geeqie

## Chrome
sudo apt -y install libxss1 libappindicator1 libindicator7  
cd $HOME/Downloads  
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb  
sudo dpkg -i google-chrome*.deb  
rm google-chrome*.deb  
cd -

## latex  
cd ~/Downloads  
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz  
tar -xzf install-tl-unx.tar.gz  
cd install-tl-*/
sudo ./install-tl  
cd ../  
rm -rf install-tl-*  

## roaster/beanbox specifics
sudo addgroup data 
sudo usrmod -a -G data coffee
sudo usrmod -a -G data root

## perl6   
sudo apt -y update
sudo apt -y install build-essential libssl-dev
cd ${HOME}/Downloads  
wget https://rakudo.org/latest/star/source  
tar xzf source  
sudo mv rakudo-star-* /usr/local/rakudo  
rm -f ${HOME}/source
cd /usr/local/rakudo
perl Configure.pl --backend=moar --gen-moar
make
make rakudo-test
make rakudo-spectest
make install
echo "export PATH=$(pwd)/install/bin/:$(pwd)/install/share/perl6/site/bin:\$PATH" >> ~/.bashrc
source ~/.bashrc

