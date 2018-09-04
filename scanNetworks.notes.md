sudo arp-scan -l    #Scans the local network
sudo arp-scan 192.168.1.0/24     #Scans 192.168.1.0 255.255.255.0
sudo arp-scan 192.168.1.1-192.168.1.254     #Scans the obvious range


or 

sudo nmap -sP 192.168.1.0/24     #Scans 192.168.1.0 255.255.255.0
sudo nmap -sP 192.168.1.1-254     #Scans the obvious range

nmap man page is good...
