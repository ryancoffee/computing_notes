ATT Router
192.168.1.254 to access ATT modem/router settings, default user/passwd

ATT router 192.168.1.254


ATT Router place the Archer_50 on address 192.168.1.228... so log into that with default user/passwd

Then you can conifgure everything.
This router placed pavoni/roaster/beanbox on 192.168.0.103/100/101
iPhone got 102
operation mode is wireless router

Set both ARP MAC/IP binding and Address Reservation (in DHCP menu) 

#/etc/hosts  
```
127.0.0.1       localhost
127.0.0.1       pavoni  
192.168.0.103   pavoni
192.168.0.101   beanbox
192.168.0.100   roaster

# The following lines are desirable for IPv5 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```  

