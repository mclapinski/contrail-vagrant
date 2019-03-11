Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "8192"
    vb.cpus = 2
  end

  config.vm.network "forwarded_port", guest: 5000, host: 50000

  ENV['LC_ALL'] = "C"

  config.ssh.forward_agent = true

  # TODO rewrite to ansible or docker
  config.vm.provision "shell", inline: <<-SHELL
    apt-add-repository ppa:opencontrail/ppa
    echo "deb [trusted=yes] http://107.170.203.108/ubuntu trusty-master main" > /etc/apt/sources.list.d/opencontrail-private.list
    apt-get update
    apt-get install -y git cmake
    apt-get install -y autoconf automake bison debhelper flex libcurl4-openssl-dev libexpat-dev libgettextpo0 libprotobuf-dev libtool libxml2-utils make protobuf-compiler python-all python-dev python-lxml python-setuptools python-sphinx ruby-ronn scons unzip vim-common libsnmp-python libipfix-dev librdkafka-dev librdkafka1
    apt-get install -y libboost-dev libboost-chrono-dev libboost-date-time-dev libboost-filesystem-dev libboost-program-options-dev libboost-python-dev libboost-regex-dev libboost-system-dev libboost-thread-dev libssl-dev google-mock libgoogle-perftools-dev liblog4cplus-dev libtbb-dev libhttp-parser-dev libxml2-dev libicu-dev python-pip
    apt-get install -y libpcap-dev libnl-genl-3-dev libnl-3-dev libzookeeper-mt-dev libzookeeper-mt2 libuv cassandra-cpp-driver cassandra-cpp-driver-dev libnuma-dev liburcu-dev libsasl2-dev
    apt-get install -y git-review
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    mkdir -p ~/bin
    curl -sS https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo
  SHELL
  
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    ssh-keyscan github.com >> ~/.ssh/known_hosts
    mkdir -p ~/contrail
    cd ~/contrail && repo init -u git@github.com:Juniper/contrail-vnc && repo sync --no-clone-bundle
    cd ~/contrail/third_party && python fetch_packages.py
  SHELL
end

