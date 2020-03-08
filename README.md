# Automation Logic Technical Test



### Summary:

This repo contains 2 branches, 

[vagrant-provision]: https://github.com/m477r1x/automation_logic/tree/vagrant-provision

 and 

[ansible-provision]: https://github.com/m477r1x/automation_logic/tree/ansible-provision



The vagrant branch provisions 2 nginx webservers and 1 nginx load balancer, all running on the same base image as instructed. The vagrant branch provisions the nginx configuration and the sudo configuration using just vagrant methods.

### Process for the vagrant-provision branch

1. Make sure you are in the automation_logic root directory and run the command `vagrant up` 
2. Wait for vagrant to provision 3 vms, which consist of standard nginx webservers, with a static hello world page configured and one nginx server with a slight modification to it's config to direct traffic between the two other vms.
3. Browse to the IP address of the loadbalancer vm (192.168.33.30). You should see the hello world page.

### Testing

In order to confirm that load balancing is happening correctly, you can 







### Learning Resources Used

https://www.vagrantup.com/docs/provisioning/ansible_intro.html

https://docs.ansible.com/ansible/latest/scenario_guides/guide_vagrant.html

https://www.vagrantup.com/docs/provisioning/ansible.html

https://www.youtube.com/watch?v=ndNrSSDR-j4

https://www.youtube.com/watch?v=RSslFGH1Vjw

https://www.vagrantup.com/docs/provisioning/file.html

https://www.vagrantup.com/docs/vagrantfile/



### Times taken:

**20 mins** - learn how to launch a basic nginx machine using vagrant
**35 mins** - learn how to launch multiple machines using a single vagrantfile, troubleshoot/research when it doesn't work and eventually put it down to Windows being pants and hope this is acceptable. Then start again the following morning and for some reason it works..... thanks Windows.
**15 mins** - learn how to manually configure an nginx load balancer and make neccassary additions to vagrantfile including folder redirection for nginx config
**1 min** - realise this causes problems because folder redirection makes package installer think there is a previous installation of nginx because of existing config files in /etc/nginx and prompts for user intervention.
**30 mins** - reading about ansible and creating a playbook to apply to the webservers and the loadbalancer and discovering that ansible is not supported on Windows hosts officially, and requires some weird tinkering of user accounts and stuff to get it working and didn't fancy fudging my custom gaming desktop.
**25 mins** - go back to basics and provision the loadbalancer config by copying a source file to destination from host to guest using vagrant natively.

**10 mins** - modify the sudoers file and add lines to copy into vms to allow vagrant user and admin user group to sudo without password prompt.