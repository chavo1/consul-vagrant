# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    config.vm.box = "chavo1/trusty64"
  
    1.upto(2) do |n|
      config.vm.define "consul0#{n}" do |box|
        box.vm.hostname = "consul#{n}"
        box.vm.network "private_network", ip: "192.168.56.5#{n}"
  
        box.vm.provision "shell", inline: <<-SCRIPT
  apt-get update
  apt-get install -y ruby ruby-dev unzip
  gem install diplomat --no-rdoc --no-ri
  gem install pry --no-rdoc --no-ri
  
  cd /tmp
  cp /vagrant/consul.zip consul.zip
  
  unzip consul.zip
  chmod +x consul
  mv consul /usr/bin/consul
  
  cp /vagrant/init-consul.conf /etc/init/consul.conf
  echo "exec consul agent -config-file=/vagrant/consul.json -advertise=192.168.56.5#{n}" | tee -a /etc/init/consul.conf
  
  mkdir /var/lib/consul
  service consul start
        SCRIPT
      end
    end
  end