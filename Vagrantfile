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

    node.vm.provision "install redis", type: "shell", run: "once", inline: <<-SHELL
      curl -fsSL https://packages.redis.io/gpg | gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

      echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/redis.list

      apt-get -y update
      apt-get -y install redis

      cp /vagrant/redis.conf /etc/redis/redis.conf
      systemctl restart redis-server
    SHELL
  end

  config.vm.define "redis-1" do |node|
    node.vm.hostname = "vm-redis-1"
    node.vm.network "private_network", ip: "192.168.56.32"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "redis-1"
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
