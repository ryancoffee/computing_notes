# General  
I am generally doing 30GB for / and the filling the rest with /home  

## partitioning with SSDs  
https://askubuntu.com/questions/289196/how-should-i-partition-my-128gb-crucial-ssd-in-my-netbook-for-ubuntu-13-04  
I stopped doing /boot in it's own partition since that hurts SSDs since they need space to move things around and the life of the drive is compromised if you don't allow it to use all of its free space to swap registers.  

## partitioning with HDDs  
https://ubuntuforums.org/showthread.php?t=2379280  
Here I like to give 2GB to /boot and at least 30gb to /   
Then one would also give a restricted amount to /etc and /tmp since those can grow large, likely with web hosting  


