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
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

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
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "1024"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.vm.provision "fix-no-tty", type: "shell" do |s|
    s.privileged = false
    s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end
  config.vm.provision "shell", inline: <<-SHELL
    sudo -i
    export DEBIAN_FRONTEND=noninteractive
    echo '---------------------------------'
    echo '----- Install CURL --------------'
    echo '---------------------------------'
    apt-get install -y curl
    echo '---------------------------------'
    echo '----- Install NodeJS ------------'
    echo '---------------------------------'
    curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
    apt-get install -y nodejs npm
    echo '---------------------------------'
    echo '-- Update apt-get and Install  --'
    echo '---------------------------------'
    apt-get update
    apt-get install -y apache2 build-essential checkinstall
    apt-get install -y libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
    apt-get install -y libmysqlclient-dev git
    apt-get install -y python-dev libxml2-dev libxslt1-dev zlib1g-dev

    echo '---------------------------------'
    echo '-- Install Maven --'
    echo '---------------------------------'
    apt-get install -y maven
    echo '---------------------------------'
    echo '-- Install Java 8 DK           --'
    echo '---------------------------------'
    apt-get install -y python-software-properties
    add-apt-repository ppa:webupd8team/java
    apt-get update
    apt-get install -y oracle-java8-installer
    echo debconf shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
    echo debconf shared/accepted-oracle-license-v1-1 seen true | /usr/bin/debconf-set-selections
    apt-get install --yes oracle-java8-installer
    yes "" | apt-get -f install
    apt-get install -y oracle-java8-set-default
    echo '---------------------------------'
    echo '-- Install Python 2.7.2        --'
    echo '---------------------------------'
    add-apt-repository ppa:fkrull/deadsnakes
    apt-get update
    apt-get install -y python2.7 python-setuptools python-pip
    echo '---- This is the Vagrant NPMRC file set up ----'
    sudo -S -u vagrant touch /home/vagrant/.npmrc && echo registry=http://futuremedia:8085/repository/npm-remote >> .npmrc
 SHELL
end
