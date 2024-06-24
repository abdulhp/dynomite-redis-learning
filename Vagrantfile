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

      vb.memory = 3072
    end

    node.vm.provision "install redis", type: "shell", run: "once", inline: <<-SHELL
      apt-get install -y make autoconf gcc libjemalloc2

      wget -P /tmp https://download.redis.io/releases/redis-5.0.14.tar.gz

      tar -xvf /tmp/redis-5.0.14.tar.gz -C /tmp

      cd /tmp/redis-5.0.14 && make

      install -m 755 /tmp/redis-5.0.14/src/redis-server /usr/local/bin/redis-server
      install -m 755 /tmp/redis-5.0.14/src/redis-cli /usr/local/bin/redis-cli
      mkdir -p /etc/redis

      cp /vagrant/redis.conf /etc/redis/redis.conf
      cp /vagrant/redis.service /etc/systemd/system/redis.service
    SHELL
  end

  config.vm.define "dyno-0" do |node|
    node.vm.hostname = "vm-dyno-0"
    node.vm.network "private_network", ip: "192.168.56.31"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "dyno-0"
      vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
      vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
      vb.customize ["modifyvm", :id, "--audio", "none"]

      vb.customize ["modifyvm", :id, "--usb", "off"]
      vb.customize ["modifyvm", :id, "--uart1", "off"]
      vb.customize ["modifyvm", :id, "--uart2", "off"]
      vb.customize ["modifyvm", :id, "--uart3", "off"]
      vb.customize ["modifyvm", :id, "--uart4", "off"]

      vb.memory = 3072
    end

    node.vm.provision "install redis", type: "shell", run: "once", inline: <<-SHELL
      apt-get install -y make autoconf gcc libjemalloc2

      wget -P /tmp https://download.redis.io/releases/redis-5.0.14.tar.gz

      tar -xvf /tmp/redis-5.0.14.tar.gz -C /tmp

      cd /tmp/redis-5.0.14 && make

      install -m 755 /tmp/redis-5.0.14/src/redis-server /usr/local/bin/redis-server
      install -m 755 /tmp/redis-5.0.14/src/redis-cli /usr/local/bin/redis-cli
      mkdir -p /etc/redis

      cp /vagrant/redis.conf /etc/redis/redis.conf
      cp /vagrant/redis.service /etc/systemd/system/redis.service
    SHELL
  end

end
