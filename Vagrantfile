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
##### CONFIGURE SHARED FOLDER #####
  web1.vm.synced_folder "./Webservers/", "/provision" 
##### INSTALL NGINX,COPY THE HELLO WORLD SITE AND CONFIGURE SUDO OVERRIDES #####
  web1.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
    echo vagrant ALL=NOPASSWD:ALL > /etc/sudoers.d/overrides
    echo %admin ALL=NOPASSWD:ALL > /etc/sudoers.d/overrides
    sudo cp /provision/hello-world.html /var/www/html/index.nginx-debian.html
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
##### CONFIGURE SHARED FOLDER #####
  web2.vm.synced_folder "./Webservers/", "/provision" 
##### INSTALL NGINX,COPY THE HELLO WORLD SITE AND CONFIGURE SUDO OVERRIDES #####  
  web2.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
    echo vagrant ALL=NOPASSWD:ALL > /etc/sudoers.d/overrides
    echo %admin ALL=NOPASSWD:ALL > /etc/sudoers.d/overrides
    sudo cp /provision/hello-world.html /var/www/html/index.nginx-debian.html
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
  ##### CONFIGURE SHARED FOLDER #####
  lb1.vm.synced_folder "./LoadBalancer/", "/provision"  
##### INSTALL NGINX, COPY LOAD BALANCER CONFIG AND RELOAD, ADD SUDOERS OVERRIDES #####
  lb1.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo cp /provision/loadbalancerconfig /etc/nginx/sites-available/default
    sudo service nginx restart
    echo vagrant ALL=NOPASSWD:ALL > /etc/sudoers.d/overrides
    echo %admin ALL=NOPASSWD:ALL > /etc/sudoers.d/overrides
  SHELL
  end
end



