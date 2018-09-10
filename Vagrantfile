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

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "mrlesmithjr/rhel-7"
  config.vm.box_version = "20160421.0"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

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
  config.vm.network "private_network", ip: "192.168.99.20", auto_config: false

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = false
  
     # Customize the amount of memory on the VM:
     vb.memory = "1536"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  #config.vm.provision "shell", inline: <<-SHELL
  #   nmcli con delete enp0s8
  #   nmcli con add type ethernet ifname enp0s8 con-name enp0s8 
  #   nmcli con mod enp0s8 ipv4.addresses 192.168.99.20/24 ipv4.gateway 192.168.99.1 ipv4.dns 192.168.99.1 ipv4.dns-search jhveras.org ipv4.method manual connection.autoconnect yes
  #   nmcli connection up enp0s8
  #   nmcli con delete enp0s3
  #SHELL

  #Multiple machines
  config.vm.define "server" do |server|
     #server.vm.box = "rhce-server"
     server.vm.provision :shell, inline: <<-SHELL
	hostnamectl set-hostname rhce-server.jhveras.org
        nmcli con delete enp0s8
        nmcli con add type ethernet ifname enp0s8 con-name enp0s8 
        nmcli con mod enp0s8 ipv4.addresses 192.168.99.20/24 ipv4.gateway 192.168.99.1 ipv4.dns 192.168.99.1 ipv4.dns-search jhveras.org ipv4.method manual connection.autoconnect yes
        nmcli connection up enp0s8
	nmcli con mod enp0s3 ipv4.ignore-auto-dns yes
     SHELL
  end

  config.vm.define "client" do |client|
     #client.vm.box = "rhce-client01"
     client.vm.provision :shell, inline: <<-SHELL
        hostnamectl set-hostname rhce-client01.jhveras.org
        nmcli con mod enp0s3 ipv4.ignore-auto-dns yes
        nmcli con del enp0s8
	nmcli con add type ethernet ifname enp0s8 con-name enp0s8 autoconnect yes
        nmcli con mod enp0s8 ipv4.addresses 192.168.99.40/24 ipv4.gateway 192.168.99.1 ipv4.dns 192.168.99.1 ipv4.dns-search jhveras.org ipv4.method manual 
        nmcli con up enp0s8	
     SHELL
  end
end
