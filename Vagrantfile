# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "webserver1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", ip: "192.168.33.10"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      vb.name = "webserver1"
  end
  web1.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
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
  web2.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
  SHELL
  end

  config.vm.define "loadbalancer" do |lb1|
    lb1.vm.box = "ubuntu/bionic64"
    lb1.vm.network "private_network", ip: "192.168.33.30"
    config.vm.synced_folder "./loadbalancer/", "/etc/nginx/"
    lb1.vm.provider "virtualbox" do |vb|
     vb.memory = 1024
     vb.cpus = 1
     vb.name = "loadbalancer"
    end
  lb1.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y nginx
    sudo service nginx start
  SHELL
  lb1.vm.provision "file", source: "./loadbalancerconfig", destination: "/etc/nginx/sites-available/default"
  end
end



