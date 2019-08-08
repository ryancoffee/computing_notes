# KubeFlow  
https://www.kubeflow.org/  

# Hybrid Clouds  
https://cloud.google.com/blog/products/gcp/simplifying-machine-learning-on-open-hybrid-clouds-with-kubeflow  

# From Chris White <cjwhite@google.com>  
Hi Ryan - see below  
On Tue, Sep 11, 2018 at 9:44 PM Coffee, Ryan <coffee@slac.stanford.edu> wrote:  
Hey, thanks for the help!  I hope we can get someone to come speak.  It might be a was to warm SLAC up to hosting inter-lab shared data at Google.  
-- Hybrid Cloud is the way to go... some optimal combination of on-prem with public Cloud infra  
 

One of my colleagues used to have an Azure research grant that gave him some access to their cloud.  He was in particle physics.  I don't, and LCLS hosts its own compute and storage inside our analysis sub-net.  Is there anything like the Azure program where we propose an academic project and then get "time grants" for the compute and storage?  
-- Not 100% sure - it may be possible to get credits for basic usage (sales question)  
 

I imagine for now we could store locally but do the compute there.  However, our goal is to compress the data on the device before transfer.  This is even the case for the fat data tube we are laying down to get data to NERSC.   

I am however getting chummy with kubernetes and docker.  
-- There is also an option called Kubeflow which can work for ML with on-prem data, and can also migrate to Cloud   

 
So I'd like to spin up some virtual boxes on your cloud and run my simulations and do some transfer learning of my own in your cloud. The trouble with your AutoML is that you are using natural images.  I get really nervous about only seeing cats if I train on a cats and then transfer to images of stars.   
-- Yes, there are to make it easier to use ML for common use cases  
 
But I have simulations that can generate ground truth for training and then expensive algorithms for getting ground truth for real data.    


## Micro k8s  
https://microk8s.io/?utm_source=MOTD&utm_medium=MOTD&utm_campaign=1)FY19_Cloud_K8
  
This is for a single node installation or on edge IoT boxes  
