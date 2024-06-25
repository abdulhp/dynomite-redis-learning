# -*- mode: ruby -*-
# vi: set ft=ruby :

DYNO_IPS = [
  '192.168.56.31',
  '192.168.56.32'
]

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

  DYNO_IPS.each_with_index do |ip, index|
    config.vm.define "dyno-#{index}" do |node|
      node.vm.hostname = "vm-dyno-#{index}"
      node.vm.network "private_network", ip: ip
  
      node.vm.provider "virtualbox" do |vb|
        vb.name = "dyno-#{index}"
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
        apt-get update -y -q 
        apt-get install -y make autoconf gcc libjemalloc2 libtool build-essential libncurses-dev bison flex libssl-dev libelf-dev
  
        wget -P /tmp https://download.redis.io/releases/redis-5.0.14.tar.gz
  
        tar -xvf /tmp/redis-5.0.14.tar.gz -C /tmp
  
        cd /tmp/redis-5.0.14 && make
  
        install -m 755 /tmp/redis-5.0.14/src/redis-server /usr/local/bin/redis-server
        install -m 755 /tmp/redis-5.0.14/src/redis-cli /usr/local/bin/redis-cli
        mkdir -p /etc/redis
  
        cp /vagrant/redis.conf /etc/redis/redis.conf
        cp /vagrant/redis.service /etc/systemd/system/redis.service

        systemctl enable redis
        systemctl start redis
      SHELL
  
      node.vm.provision "install dynomite", type: "shell", run: "once", inline: <<-SHELL
        cd ~
        git clone http://github.com/Netflix/dynomite.git
        cd ./dynomite
        autoreconf -fvi
        ./configure --enable-debug=yes
        make
        src/dynomite -h
  
        install -m 755 src/dynomite /usr/local/sbin/dynomite
  
        mkdir /etc/dynomite
        mkdir /etc/dynomite/conf
  
        cp ~/dynomite/conf/dynomite.pem /etc/dynomite/conf/dynomite.pem
        cp /vagrant/dyno-#{index}.yml /etc/dynomite/dynomite.yaml
  
        cp /vagrant/dynomite.service /etc/systemd/system/
  
        systemctl start dynomite
      SHELL
  
    end
  end

  # config.vm.define "dyno-0" do |node|
  #   node.vm.hostname = "vm-dyno-0"
  #   node.vm.network "private_network", ip: "192.168.56.31"

  #   node.vm.provider "virtualbox" do |vb|
  #     vb.name = "dyno-0"
  #     vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
  #     vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
  #     vb.customize ["modifyvm", :id, "--audio", "none"]

  #     vb.customize ["modifyvm", :id, "--usb", "off"]
  #     vb.customize ["modifyvm", :id, "--uart1", "off"]
  #     vb.customize ["modifyvm", :id, "--uart2", "off"]
  #     vb.customize ["modifyvm", :id, "--uart3", "off"]
  #     vb.customize ["modifyvm", :id, "--uart4", "off"]

  #     vb.memory = 3072
  #   end

  #   node.vm.provision "install redis", type: "shell", run: "once", inline: <<-SHELL
  #     apt-get install -y make autoconf gcc libjemalloc2 libtool build-essential libncurses-dev bison flex libssl-dev libelf-dev

  #     wget -P /tmp https://download.redis.io/releases/redis-5.0.14.tar.gz

  #     tar -xvf /tmp/redis-5.0.14.tar.gz -C /tmp

  #     cd /tmp/redis-5.0.14 && make

  #     install -m 755 /tmp/redis-5.0.14/src/redis-server /usr/local/bin/redis-server
  #     install -m 755 /tmp/redis-5.0.14/src/redis-cli /usr/local/bin/redis-cli
  #     mkdir -p /etc/redis

  #     cp /vagrant/redis.conf /etc/redis/redis.conf
  #     cp /vagrant/redis.service /etc/systemd/system/redis.service
  #   SHELL

  #   node.vm.provision "install dynomite", type: "shell", run: "once", inline: <<-SHELL
  #     cd ~
  #     git clone http://github.com/Netflix/dynomite.git
  #     cd ./dynomite
  #     autoreconf -fvi
  #     ./configure --enable-debug=yes
  #     make
  #     src/dynomite -h

  #     install -m 755 /src/dynomite /usr/local/bin/dynomite

  #     mkdir /etc/dynomite
  #     mkdir /etc/dynomite/conf

  #     cp ~/dynomite/conf/dynomite.pem /etc/dynomite/conf/dynomite.pem
  #     cp /vagrant/dyno-0.yml /etc/dynomite/dynomite.yaml

  #     cp /vagrant/dynomite.service /etc/systemd/system/

  #     systemctl start dynomite
  #   SHELL

  # end

end
