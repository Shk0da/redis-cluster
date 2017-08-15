# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	
  # proxyconf
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = "http://monitoring.eastwind.ru:3129"
    config.proxy.https    = "http://monitoring.eastwind.ru:3129"
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end

  # ubuntu 64bit image
  config.vm.box = "ubuntu-trusty"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  # enable guest auto updates
  unless Vagrant.has_plugin?("vagrant-vbguest")
    raise 'vagrant-vbguest is not installed! run: "vagrant plugin install vagrant-vbguest"'
  end
  config.vbguest.auto_update = true
  
  config.vm.provider :virtualbox do |vb|
      #vb.gui = true
      # hacky fix for trouble with ipv6 and dns network timeout
      # https://github.com/mitchellh/vagrant/issues/1172
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end
  
  # install the redis server
  config.vm.provision :shell, :path => "bootstrap.sh"
  config.vm.provision :shell, :path => "build_redis.sh"
  config.vm.provision :shell, :path => "install_redis.sh"

  # setup forwarded ports
  config.vm.network "forwarded_port", guest: 7000, host: 7000
  config.vm.network "forwarded_port", guest: 7001, host: 7001
  config.vm.network "forwarded_port", guest: 7002, host: 7002
  config.vm.network "forwarded_port", guest: 7003, host: 7003
  config.vm.network "forwarded_port", guest: 7004, host: 7004
  config.vm.network "forwarded_port", guest: 7005, host: 7005
end
