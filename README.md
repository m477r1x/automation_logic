# Automation Logic Technical Test



### Summary:

This repo contains 2 branches vagrant-provision and ansibpe-provision. 

The vagrant branch provisions 2 nginx webservers and 1 nginx load balancer, all running on the same base image as instructed. The vagrant branch provisions the nginx configuration and the sudo configuration using just vagrant methods.

### Process for the ansible-provision branch

1. Make sure you are in the automation_logic root directory and run the command `vagrant up` 
2. Wait for vagrant to provision 3 vms, which consist of 3 standard virtual machines running the specified base image <u>ubuntu/bionic64</u>. The VMs are *webserver1*, *webserver2* and *loadbalancer*
3. Run the command `vagrant ssh loadbalancer`, which will open an SSH session to the loadbalancer vm. To confirm that nginx is not configured on any of the boxes you can run `curl` for all three IP addresses of the vms, 192.168.33.10/20/30 to confirm you should receive `curl: (7) Failed to connect to 192.168.1.10 port 80: Connection timed out` or similar.
4. make sure you are in the root directory of the repo and run `vagrant up` this should bring up 3 vms similar to the vagrant-provision branch, but this time ansible will initialise and attempt to provision the machines. you can see the difference in vm configuration in the vagrantfile in this branch which has ansible configured, and does not contain any nginx references.

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

https://code-maven.com/install-and-configure-nginx-using-ansible

https://codelike.pro/how-to-configure-nginx-using-ansible-on-ubuntu/

### Times taken:

**20 mins** - learn how to launch a basic nginx machine using vagrant
**35 mins** - learn how to launch multiple machines using a single vagrantfile, troubleshoot/research when it doesn't work and eventually put it down to Windows being pants and hope this is acceptable. Then start again the following morning and for some reason it works..... thanks Windows.

**15 mins** - learn how to manually configure an nginx load balancer and make neccassary additions to vagrantfile including folder redirection for nginx config

**1 min** - realise this causes problems because folder redirection makes package installer think there is a previous installation of nginx because of existing config files in /etc/nginx and prompts for user intervention.

**20 mins** - configure hello world page, and copy operations for the web servers to serve the page.

**30 mins** - reading about ansible and creating a playbook to apply to the webservers and the loadbalancer and discovering that ansible is not supported on Windows hosts officially, and requires some weird tinkering of user accounts and stuff to get it working and didn't fancy fudging my custom gaming desktop.
**25 mins** - go back to basics and provision the loadbalancer config by copying a source file to destination from host to guest using vagrant natively.

**20 mins** - read up on best practices on modifying sudoers file and implement updated config for admin group and vagrant user

**45 mins** - ~~dicking~~ tinkering about manually spinning up ubuntu VM to get around windows problems and attempt ansible configuration but unfortunately as I anticipated, vagrant isn't very good at bringing up VMs on a VM.

**1 hour** - researching ways to automate testing nginx loadbalancing but unable to find anything which did not involve learning PHP, so instead opted for a simple check by having different configurations on each webserver so when you request the page, you can see which server the response is coming from by means of the page title.

**1 hour** - write documentation
