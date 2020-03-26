# GenICam

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub
sudo dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
sudo apt-get update
sudo apt-get -y install cuda
```

# coaxlink
```bash
tar -xzf memento-linux-x86_64-12.4.0.8.tar.gz
cd memento-linux-x86_64-12.4.0.8/
sudo apt install apt-file dkms
sudo apt update
/opt/euresys/memento/shell/check-install.sh
sudo ./install.sh
cd ../coaxlink-linux-x86_64-12.4.0.8/
sudo apt install libgconf-2-4 libtinfo5
sudo apt update
/opt/euresys/coaxlink/shell/check-install.sh
sudo ./install.sh
. /opt/euresys/coaxlink/shell/setup_gentl_paths.sh
```





