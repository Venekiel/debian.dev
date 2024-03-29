# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.define "deb11-dev-vagrant" do |config|
    config.vm.hostname = "deb11-dev-vagrant"

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://vagrantcloud.com/search.
    config.vm.box = "debian/bullseye64"

    # The url from where the 'config.vm.box' box will be fetched if it
    # doesn't already exist on the user's system.
    config.vm.box_url = "debian/bullseye64"

    config.vm.post_up_message = "VM deb11-dev-vagrant Ok"
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.56.103"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "../../Projets", "/data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "4096"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with chef solo, specifying a cookbook path, roles
  # path, and data_bags path (all relative to this vagrantfile), and adding
  # some recipes and/or roles.
  #
  # config.berkshelf.enabled = true
  # config.vm.provision :chef_solo do |chef|
  #  chef.add_recipe "apt"
  #   chef.add_recipe "docker"
  #   chef.add_recipe "git"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # 
  # Install latest version of git and docker-compose (including docker)
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y git
    apt-get install -y docker-compose
  SHELL

  # Starting-up the project after VM boot-up
  config.trigger.after :up do |trigger|
    trigger.name = "Project start-up"
    trigger.info = "Starting Servlin project..."
    trigger.run_remote = {
      inline: <<-SHELL
        cd /data/Servlin
        docker-compose up -d
      SHELL
    }
  end

  # Stopping the project after VM halt
  config.trigger.before :halt do |trigger|
    trigger.name = "Project stop"
    trigger.info = "Stopping Servlin project..."
    trigger.run_remote = {
      inline: <<-SHELL
        cd /data/Servlin
        docker-compose down
      SHELL
    }
  end
  
end
