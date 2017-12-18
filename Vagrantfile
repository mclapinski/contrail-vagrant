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
    # Display the VirtualBox GUI when booting the machine
    #vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "8192"
    vb.cpus = 2
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

  config.vm.provision "file", source: "~/.ssh/other_keys/vms_key", destination: ".ssh/id_rsa"
  config.vm.provision "file", source: "~/.ssh/other_keys/vms_key.pub", destination: ".ssh/id_rsa.pub"
  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-add-repository ppa:opencontrail/ppa
    apt-get update
    apt-get install -y git cmake
    apt-get install -y autoconf automake bison debhelper flex libcurl4-openssl-dev libexpat-dev libgettextpo0 libprotobuf-dev libtool libxml2-utils make protobuf-compiler python-all python-dev python-lxml python-setuptools python-sphinx ruby-ronn scons unzip vim-common libsnmp-python libipfix-dev librdkafka-dev librdkafka1
    apt-get install -y libboost-dev libboost-chrono-dev libboost-date-time-dev libboost-filesystem-dev libboost-program-options-dev libboost-python-dev libboost-regex-dev libboost-system-dev libcurl4-openssl-dev google-mock libgoogle-perftools-dev liblog4cplus-dev libtbb-dev libhttp-parser-dev libxml2-dev libicu-dev
    apt-get install -y libpcap-dev libnl-genl-3-dev libnl-3-dev libzookeeper-mt-dev libzookeeper-mt2
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    mkdir -p ~/bin
    curl -sS https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo
  SHELL
  
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    ssh-keyscan github.com >> ~/.ssh/known_hosts
    mkdir -p ~/contrail-vrouter
    cd ~/contrail-vrouter && repo init -u git@github.com:Juniper/contrail-vnc && repo sync
    cd ~/contrail-vrouter/third_party && python fetch_packages.py
    mkdir -p ~/contrail-vrouter/third_party/cassandra
    cd ~/contrail-vrouter/third_party/cassandra && wget https://downloads.datastax.com/cpp-driver/ubuntu/14.04/cassandra/v2.2.0/cassandra-cpp-driver_2.2.0-1_amd64.deb && wget https://downloads.datastax.com/cpp-driver/ubuntu/14.04/cassandra/v2.2.0/cassandra-cpp-driver-dev_2.2.0-1_amd64.deb && wget https://downloads.datastax.com/cpp-driver/ubuntu/14.04/dependencies/libuv/v1.7.5/libuv_1.7.5-1_amd64.deb && sudo dpkg -i libuv_1.7.5-1_amd64.deb cassandra-cpp-driver_2.2.0-1_amd64.deb cassandra-cpp-driver-dev_2.2.0-1_amd64.deb
  SHELL
end

