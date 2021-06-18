# SSH services  
http://www.portchecktool.com/  

sudo service ssh status  
to check the status of ssh daemon running on the machine we are trying to get into  

sudo service ssh start  
to get ssh started on that machine.  


https://unix.stackexchange.com/questions/12755/how-to-forward-x-over-ssh-to-run-graphics-applications-remotely  

Got it working.  but still have to have computer on and issue a   
sudo service ssh start  

## ssh-keygen  
For now I logged into root  
this is largely because I will eventually like to mirror my /home directories between the two machines.  
did an ssh-keygen  
then copy the .pub key from there to the machine I want to be able to get into it.    
ssh-keygen   
ssh-add ~/.ssh/id_rsa_pavoni

## settting up ssh ##  
add this to the .bashrc
```bash
if [ ! -S ~/.ssh/ssh_auth_sock ]; then
  eval `ssh-agent`
  ln -sf "$SSH_AUTH_SOCK" ~/.ssh/ssh_auth_sock
fi
export SSH_AUTH_SOCK=~/.ssh/ssh_auth_sock
ssh-add -l | grep "The agent has no identities" && ssh-add .ssh/id_rsa_pavoni
```

choose file: /home/coffee/.ssh/id_rsa_beanbox  
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"  
choose file: /home/coffee/.ssh/id_rsa_git  
add the .pub key from the _git to your Github keys  

## ports  
```
https://askubuntu.com/questions/144364/ssh-connect-to-host-myremotehost-com-port-22-connection-refused/406436  
also good tricks, mostly for the  
sudo service ssh status  
sudo service ssh start  
```
To open a port between two computers use netcat utility or nc
pavoni: nc -l 5000  
roaster: cat - | nc pavoni 5000  

If setting the port to something other than 22, then getting in means  
```
ssh -L localport:127.0.0.1:localport username@ServerIP **-p** **serverport**  
something like this.  
```

## No-IP  
```bash
cd /usr/local/src/
sudo wget https://www.noip.com/client/linux/noip-duc-linux.tar.gz  
sudo tar xzf noip-duc-linux.tar.gz
cd noip-2.1.9*
make
make install
sudo /usr/local/bin/noip2
```

## AFS versus NFS  
I think I'm going to want AFS rather than NFS in order to best use the fast SDD in beanbox and roaster
while keeping the data also safe in /home and /data on the TB drive in pavoni
	https://en.wikipedia.org/wiki/Andrew_File_System
	https://en.wikipedia.org/wiki/Network-attached_storage
	https://en.wikipedia.org/wiki/Network_File_System




