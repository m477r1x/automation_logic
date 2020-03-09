# Automation Logic Technical Test



### Summary:

This repo contains 2 branches vagrant-provision and ansible-provision. 

The vagrant branch provisions 2 nginx webservers and 1 nginx load balancer, all running on the same base image as instructed. The vagrant branch provisions the nginx configuration and the sudo configuration using just vagrant methods.

Since I had no prior knowledge of using vagrant or ansible, some of the time required me to research configuration and syntax for the tools.

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
**20 mins** - learn how to manually configure an nginx load balancer and make neccassary additions to vagrantfile including folder redirection for nginx config
**1 min** - realise this causes problems because folder redirection makes package installer think there is a previous installation of nginx because of existing config files in /etc/nginx and prompts for user intervention.

**20 mins** - configure hello world page, and copy operations for the web servers to serve the page.

**30 mins** - reading about ansible and creating a playbook to apply to the webservers and the loadbalancer and discovering that ansible is not supported on Windows hosts officially, and requires some weird tinkering of user accounts and stuff to get it working and didn't fancy fudging my custom gaming desktop.
**90 mins** - go back to basics and provision the loadbalancer config by copying a source file to destination from host to guest using vagrant natively, create a vagrant-provision git branch and work on this for the rest of the config until I can use a linux based system for ansible. Troubleshoot issues with deployment and code syntax until working correctly.

**20 mins** - read up on best practices on modifying sudoers file and implement updated config for admin group and vagrant user using sudoers.d directory rather than editing the main sudoers file.

**90 mins** - ~~dicking~~ tinkering about manually spinning up ubuntu VM to get around windows problems and attempt ansible configuration but unfortunately as I anticipated, vagrant isn't very good at bringing up VMs on a VM.

**60 mins** - researching ways to automate testing nginx loadbalancing but unable to find anything which did not involve learning PHP, so instead opted for a simple check by having different configurations on each webserver so when you request the page, you can see which server the response is coming from by means of the page title.

**45 mins** - write documentation

### Feedback

On the whole I enjoyed working on this test, I managed to find time in between house renovations to work on it and it actually provided me with some time to focus on something else I enjoy as a break from building work. Although it seems like it won't take long on the face of it, once you get into it you realise that actually there are a lot of caveats and gotchas around this, especially if you are using a Windows machine. I probably could have worked out a way to get ansible working on my machine at home but I didn't really have enough time left to do that on top of finishing the test to the best of my current ability. 

This is probably one of the better technical tests which I have done during my interview process for various companies, as most other ones I have done are simply something along the lines of write some terraform to create a loadbalancer, EC2 instance, ASG, etc. And this can pretty much be done almost by copying/pasting from Hashicorp docs. Granted you need to demonstrate correct management of state files but apart from that, these tests are quite simple. This test on the other hand was challenging for me, particularly because it forced me to use tools which I had not used before, and I think that would probably be the case for many mid-level or junior DevOps engineers who have been trained mainly in docker and containers rather than VMs. 

The only thing I would say is that the test does depend on people either having access to a Mac or a Linux machine, which while this can be achieved through VMs if you have the hardware for it, it's difficult to then provision more VMs running on a VM if you see what I mean. 

### Thanks!

Thank you for allowing me to attempt this test and considering me as a candidate. You have been very accommodating to me and really have tried to enable me to complete the test as best I can by letting me use a company laptop for a short time so I very much appreciate this. Whether this submission is deemed a pass or a fail, I'm glad to have had the opportunity to give it a go, and also I now have some basic knowledge of Vagrant and ansible which I could put on my CV for next time!