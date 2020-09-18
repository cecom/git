# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure("2") do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  
  #default setup from beginning
  config.vm.box = "centos/7"
  config.vm.box_version = "1801.02"
  
  #my own setup, so not everything is build from ground
  #config.vm.box = "my-box2"
  #config.ssh.username = "vagrant"
  #config.ssh.private_key_path = "privkey"

  config.vm.hostname = 'schulung'
  config.ssh.forward_x11 = true
  config.ssh.forward_agent = true
  
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true
  
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port


  #App Our
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  #jenkins
  config.vm.network "forwarded_port", guest: 8070, host: 8070
  #gerrit
  config.vm.network "forwarded_port", guest: 8090, host: 8090
  #gerrit ssh
  config.vm.network "forwarded_port", guest: 29418, host: 29418
  #sonar
  config.vm.network "forwarded_port", guest: 9000, host: 9000
  #mariadb
  config.vm.network "forwarded_port", guest: 3306, host: 3306

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  config.vm.network "public_network"
  #config.vm.network "public_network", bridge: ...
  #Todo:
  #  vagrant ssh
  #  ifconfig
  #  ip dann nutzen

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/vagrant"
  #config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
        
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM and CPUs
    vb.name = "bm_schulung"
    vb.memory = "4096"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--vram", "96"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum --exclude="*.i386" -y update
  
    # install ansible if not allready installed
    hash ansible 2>/dev/null || yum -y install https://releases.ansible.com/ansible/rpm/release/epel-7-x86_64/ansible-2.6.6-1.el7.ans.noarch.rpm
     
    # install the environment
    cd /vagrant/bootstrap
    ansible-playbook -i hosts bootstrap.yml
  SHELL

end
