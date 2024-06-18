# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.vm.define "redis-0" do |node|
    node.vm.hostname = "vm-redis-0"
    node.vm.network "private_network", ip: "192.168.56.31"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "redis-0"
      vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
      vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
      vb.customize ["modifyvm", :id, "--audio", "none"]
      
      vb.customize ["modifyvm", :id, "--usb", "off"]
      vb.customize ["modifyvm", :id, "--uart1", "off"]
      vb.customize ["modifyvm", :id, "--uart2", "off"]
      vb.customize ["modifyvm", :id, "--uart3", "off"]
      vb.customize ["modifyvm", :id, "--uart4", "off"]
    end
  end

end
