# -*- mode: ruby -*-
# vi: set ft=ruby :

##### DEIFINE VM PARAMETERS #####
Vagrant.configure("2") do |config|
  config.vm.define "webserver1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", ip: "192.168.33.10"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.name = "webserver1"
  end
##### CONFIGURE SHARED FOLDER AND INSTALL NGINX #####
  web1.vm.synced_folder "./Webservers/", "/provision" 
  web1.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
# REPLACE SUDOERS FILE WITH UPDATED CONFIG TO REMOVE PASSWORD PROMPT
    sudo "cp /provision/sudoers /etc/"
  SHELL
  end

  config.vm.define "webserver2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.network "private_network", ip: "192.168.33.20"
    web2.vm.provider "virtualbox" do |vb|
     vb.memory = 1024
     vb.cpus = 1
     vb.name = "webserver2"
  end
##### CONFIGURE SHARED FOLDER AND INSTALL NGINX #####
  web2.vm.synced_folder "./Webservers/", "/provision" 
  web2.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
##### REPLACE SUDOERS FILE WITH UPDATED CONFIG TO REMOVE PASSWORD PROMPT #####
    sudo "cp /provision/sudoers /etc/"
  SHELL
  end

  config.vm.define "loadbalancer" do |lb1|
    lb1.vm.box = "ubuntu/bionic64"
    lb1.vm.network "private_network", ip: "192.168.33.30"
    lb1.vm.provider "virtualbox" do |vb|
     vb.memory = 1024
     vb.cpus = 1
     vb.name = "loadbalancer"
    end
  lb1.vm.synced_folder "./LoadBalancer/", "/provision"  
  lb1.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo "cp /provision/loadbalancerconfig /etc/nginx/sites-available/default"
##### RELOAD NGINX WITH NEW CONFIG FOR LOAD BALANCING #####
    sudo service nginx restart
##### REPLACE SUDOERS FILE WITH UPDATED CONFIG TO REMOVE PASSWORD PROMPT #####
    sudo "cp /provision/sudoers /etc/"
  SHELL
  end
end



