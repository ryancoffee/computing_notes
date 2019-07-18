## clean format  

```bash
sudo dd if=/dev/zero of=/dev/sdc bs=4k status=progress && sync  
3915777+0 records in
3915776+0 records out
16039018496 bytes (16 GB, 15 GiB) copied, 1451.86 s, 11.0 MB/s

sudo fdisk /dev/sdc
```
m for help (list menu)  
F list free unpartitioned space  

```bash
lsblk
sudo mkfs.ext3 -v /dev/sdc1
```

The other option is to just use the ubuntu disk utility... it's easier

Then change chmod 777 /media/disklabel

