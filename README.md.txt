# Automation Logic Technical Test



### Summary:

This repo contains 2 branches vagrant-provision and ansibpe-provision. 

The vagrant branch provisions 2 nginx webservers and 1 nginx load balancer, all running on the same base image as instructed. The vagrant branch provisions the nginx configuration and the sudo configuration using just vagrant methods.

### Process for the ansible-provision branch

1. Make sure you are in the automation_logic root directory and run the command `vagrant up` 
2. Wait for vagrant to provision 4 vms, which consist of 3 standard virtual machines running the specified base image <u>ubuntu/bionic64</u>. The VMs are *webserver1*, *webserver2*, *loadbalancer* and *testclient1*.
3. Run the command `vagrant ssh testclient1` which will start an ssh session on the testclient1 vm.

### Testing

In order to confirm that load balancing is happening correctly, you can either:

1. Whilst logged into the SSH session on testclient1, run the command `tail healthcheck.log` and you should be able to see the load balancer directing traffic between the two web servers
2. Alternatively you can run the command `vagrant ssh loadbalancer` and you can then `tail /etc/nginx/access.log` to watch the requests being directed.

### Problems encountered:

I encountered quite a few problems which needed working around on the basis that I was using a Windows machine. I was not able to finish the ansible portion of the test at home and so I have done as much as I could using an automation_logic laptop on site at the office. Another issue to note is the `lb_healthcheck` script which I wrote, works fine if you create the script on the vm itself, but when it is saved in the repo on my local Windows machine, something happens to it to do with formatting. Then when vagrant copies the file onto the testclient1 machine, and tries to run it - it receives `-bash: ./lb_healthcheck: /bin/bash^M: bad interpreter: No such file or directory`

You can work around this by simply deleting the script and re-writing it on the vm itself using vi etc, but this doesn't help the fact that it is meant to be an automated test. Hopefully when I get the chance to use a linux based machine at the office I can correct this and at least this problem will be sorted.



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

**45 mins** - Decided to include a test client machine in the setup to easily test load balancing output to avoid local host networking configuration problems which could arise depending on where this environment is being spun up on. Also configured automated script to output to healthcheck file to show requests being directed between the web servers by the load balancer

**30 mins** - write documentation