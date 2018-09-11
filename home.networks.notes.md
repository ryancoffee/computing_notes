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

## ports
https://askubuntu.com/questions/144364/ssh-connect-to-host-myremotehost-com-port-22-connection-refused/406436
also good tricks, mostly for the
sudo service ssh status
sudo service ssh start

If setting the port to something other than 22, then getting in means
ssh -L localport:127.0.0.1:localport username@ServerIP **-p** **serverport**
something like this.

