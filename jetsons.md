##setup  

Download the image file (unzip it if necessary)  

```bash
sudo apt install -y curl
curl -1sLf 'https://dl.cloudsmith.io/public/balena/etcher/setup.deb.sh' | sudo -E bash
sudo apt -y update
sudo ln -sf /opt/balenaEtcher /opt/balena-etcher-electron
sudo apt -y install balena-etcher-electron
```

## Command Line Interface  
install SD card on working computer  

```bash
lsblk
dmesg | grep mmc
```
to see where it is 
```bash
/usr/bin/unzip -p ./jetson-nano-2gb-jp451-sd-card-image.zip | sudo /bin/dd of=/dev/mmcblk0p1 bs=1M status=progress
```
didn't seem to need to be ejected.


cinema:for moovies

```bash
cat - >> get_widevine.bash
#!/bin/bash
LATEST=`curl https://dl.google.com/widevine-cdm/current.txt`
wget https://dl.google.com/widevine-cdm/$LATEST-linux-x64.zip
unzip $LATEST-linux-x64.zip
sudo mkdir /usr/lib/chromium
sudo mv libwidevinecdm.so /usr/lib/chromium
sudo chmod 644 /usr/lib/chromium/libwidevinecdm.so
```
