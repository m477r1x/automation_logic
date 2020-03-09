# Automation Logic Technical Test

### 

### Summary:

This repo contains 2 branches vagrant-provision and ansibpe-provision.

The vagrant branch provisions 2 nginx webservers and 1 nginx load  balancer, all running on the same base image as instructed. The vagrant  branch provisions the nginx configuration and the sudo configuration  using just vagrant methods.

### 

### Process for the ansible-provision branch

1. Make sure you are in the automation_logic root directory and run the command `vagrant up`
2. Wait for vagrant to provision 3 vms, which consist of 3 standard  virtual machines running the specified base image ubuntu/bionic64. The  VMs are *webserver1*, *webserver2*, loadbalancer and *testclient1*. You should see ansible hopefully run on these machines during provisioning when running `vagrant up` on this branch of the repo.

### 

### Testing

In order to confirm that load balancing is happening correctly, you can either:

1. Whilst logged into the SSH session on testclient1, run the command `tail healthcheck.log` and you should be able to see the load balancer directing traffic between the two web servers
2. Alternatively you can run the command `vagrant ssh loadbalancer` and you can then `tail /etc/nginx/access.log` to watch the requests being directed.

### 

### Problems encountered:

I encountered quite a few problems which needed working around on the basis that I was using a Windows machine. I was not able to finish the  ansible portion of the test at home and so I have done as much as I  could using an automation_logic laptop on site at the office. Another  issue to note is the `lb_healthcheck` script which I wrote,  works fine if you create the script on the vm itself, but when it is  saved in the repo on my local Windows machine, something happens to it  to do with formatting. Then when vagrant copies the file onto the  testclient1 machine, and tries to run it - it receives `-bash: ./lb_healthcheck: /bin/bash^M: bad interpreter: No such file or directory`

You can work around this by simply deleting the script and re-writing it on the vm itself using vi etc, but this doesn't help the fact that  it is meant to be an automated test. Hopefully when I get the chance to  use a linux based machine at the office I can correct this and at least  this problem will be sorted.

### 

### Problems running on other machines:

Whilst I was at the AL office on Monday, I tried to run the project  on Chris Bennet's laptop I encountered a problem whereby Vagrant would  not run because it wanted the user UID to be the same as the one which  created the VMs in the first place. Because I am using a Windows machine this is set to 0. When you run `vagrant up` you might get the same error, in which case it will tell you what your user UID is, you need to replace the zero in the file `.vagrant/machines//virtualbox/creator_uid`. Make sure you do this for all the machines. Then run `vagrant up` again.

Another issue I encountered while at the AL office was that I tried  to run my ansible branch configuration on Chris' laptop, but encountered a kernel driver error when VirtualBox tried to bring up the virtual  networking interface of webserver1. After troubleshooting for a few  mins, I decided to try to manually spin up just a plain Linux VM in  VirtualBox manually, and I got the same error. So this appears to be a  problem on Chris' laptop, not with my test configuration (sorry Chris).  Because of this, I was unable to test my ansible branch or make any  changes to it, so thus far the ansible branch of the project is largely  untested and I do not expect it to work correctly as I cannot configure  it at home and the AL laptop I was using did not allow for this either.

### 

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

### 

### Times taken:

**20 mins** - learn how to launch a basic nginx machine using vagrant **35 mins** - learn how to launch multiple machines using a single vagrantfile, troubleshoot/research when it doesn't work and  eventually put it down to Windows being pants and hope this is  acceptable. Then start again the following morning and for some reason  it works..... thanks Windows. **20 mins** - learn how to manually configure an nginx load balancer and make neccassary additions to vagrantfile including folder  redirection for nginx config **1 min** - realise this causes problems because folder  redirection makes package installer think there is a previous  installation of nginx because of existing config files in /etc/nginx and prompts for user intervention.

**20 mins** - configure hello world page, and copy operations for the web servers to serve the page.

**30 mins** - reading about ansible and creating a  playbook to apply to the webservers and the loadbalancer and discovering that ansible is not supported on Windows hosts officially, and requires some weird tinkering of user accounts and stuff to get it working and  didn't fancy fudging my custom gaming desktop. **90 mins** - go back to basics and provision the  loadbalancer config by copying a source file to destination from host to guest using vagrant natively, create a vagrant-provision git branch and work on this for the rest of the config until I can use a linux based  system for ansible. Troubleshoot issues with deployment and code syntax  until working correctly.

**20 mins** - read up on best practices on modifying  sudoers file and implement updated config for admin group and vagrant  user using sudoers.d directory rather than editing the main sudoers  file.

**90 mins** - ~~dicking~~ tinkering about  manually spinning up ubuntu VM to get around windows problems and  attempt ansible configuration but unfortunately as I anticipated,  vagrant isn't very good at bringing up VMs on a VM.

**60 mins** - researching ways to automate testing nginx loadbalancing but unable to find anything which did not involve  learning PHP, so instead opted for a simple check by having different  configurations on each webserver so when you request the page, you can  see which server the response is coming from by means of the page title.

**90 mins** - write documentation

### 

### Feedback

This technical test was one of the better ones I have undertaken  during the course of interviews. It challenged me to use tools that I  had no previous experience in. Most other tests I have done have usually consisted of creating a docker container image and deploying it into a  K8s cluster, or writing some simple Terraform code to spin up a load  balancer, ASG, EC2 instance etc. Apart from demonstrating a working  knowledge of state file management, these tests can largely be completed by copy/pasting from Hashicorp documentation. So in light of this, even if this attempt is not deemed as passable, at least I have gained some  rudimentary working knowledge of Vagrant and Ansible, so that's a plus!

The only down side of the test I would say is the fact that it  requires usage of a tool not supported on Windows and I don't have a Mac or lunix native machine at home. Which means that other people might  have a similar issue to me. Whenever I need a Linux environment I  usually spin up a VM but VMs don't usually like trying to create  VM-inception environments.

### 

### Thanks!

Thank you for allowing me to attempt this test and considering me as a candidate. You have been very accommodating to me and really have tried to enable me to complete the test as best I can by letting me use a  company laptop for a short time so I very much appreciate this. Whether  this submission is deemed a pass or a fail, I'm glad to have had the  opportunity to give it a go, and also I now have some basic knowledge of Vagrant and ansible which I could put on my CV for next time!