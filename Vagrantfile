# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.synced_folder ".", "/vagrant", disabled: true

  #Load Balancer
  config.vm.define :lb do |lb|
      lb.vm.box = "geerlingguy/centos7"
      lb.vm.hostname = "lb"
      lb.vm.network :private_network, ip: "192.168.0.100"
      lb.vm.network "forwarded_port", guest: 80, host: 8080
      lb.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
  end

  # Database
  config.vm.define :db do |db|
      db.vm.box = "geerlingguy/centos7"
      db.vm.hostname = "lb"
      db.vm.network :private_network, ip: "192.168.0.200"
      db.vm.network "forwarded_port", guest: 80, host: 8080
      db.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
      end
  end

  # Apps
  (1..2).each do |i|
    config.vm.define "app#{i}" do |app|
        app.vm.box = "geerlingguy/centos7"
        app.vm.hostname = "web#{i}"
        app.vm.network :private_network, ip: "192.168.0.30#{i}"
        app.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        app.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
    end
  end

end
