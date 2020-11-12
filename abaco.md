## Download the BSP
sudo apt install cabextract
www.4dsp.com/4FM/BSP
Probably also need the 'UnitAPI User Manual'
descend into the folder where you extract the .zip file
run 
```bash
cabextract LINUX.cab
sudo apt install linux-headers-$(uname -r)
sudo apt install libtool
sudo apt install gcc g++
tar xvzf _fm.bsp.14.09.2020.tar.gz
cd 4fm.bsp.14.09.2020/
tar xzvf 4fm-driver-64bit-0.1.2.10.tar.gz
cd 4fm-driver-0.1.2.10/
vim README.md
make
sudo make install
sudo insmod 4fm.ko
sudo depmod -a
sudo modprobe 4fm
dmesg
lsmod | grep 4fm
ls /dev/
sudo chmod 666 /dev/pc821-0
sudo vim /etc/udev/rules.d/70-snap.core.rules
```
add 
```
## this is for abaco
KERNEL=="pc821*", NAME="4dsp/%k", MODE="0666"
```

