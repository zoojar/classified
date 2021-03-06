#Lab for automating classification with node_manager

###Requirements
Virtualbox 5.0.0  
Vagrant 1.7.4  
Vagrant-Reload Vagrant plugin 0.0.1  


###Getting started  
1. Clone this repo  
2. Run:   
```
vagrant up

```



###Config

####Initial Provisioning
Vagrant uses the provisioning script 'roosters':  
https://github.com/zoojar/openstack-lab/blob/master/roosters  
  

#####Puppet Master  
This script configures the following on the puppet master:  
1. '/etc/hosts' file  
2. Disable firewall  
3. Download PE installer tarball  
4. Download PE answers file from this repo  
5. Install puppet using the answers file  
6. Download hiera.yaml config file to 'hiera_config' filepath  
7. Configures r10k using puppet module 'zack/r10k'  
8. Refreshes the classes on the master (post r10k run)  
9. Creates nodes groups specified in the puppet module 'zj/classifier' (deployed by r10k) using the puppet module 'prosvcs/node_manager'  
10. Sets up autosigning using 'autosign.sh' and a global psk to allow auto-signing of the openstack node  
  