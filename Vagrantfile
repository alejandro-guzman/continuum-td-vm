# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "continuum-vm"
  config.vm.network "forwarded_port", guest: 8080, host: 9000
  config.vm.network "public_network"
  config.vm.synced_folder "~/docker-continuum", "/docker-continuum"
  config.vm.provider "virtualbox" do |vb|
    vb.name = "Continuum VM"
    vb.memory = "4096"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
  config.vm.provision "shell", inline: <<-SHELL
    echo "Running provision..."
    ufw disable
    # ----------------
    # Docker
    # ----------------
    apt-get remove docker docker-engine docker.io containerd runc
    apt-get update
    apt-get install -y \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg-agent \
      software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    apt-key fingerprint 0EBFCD88
    add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
    apt-get update
    apt-get install -y docker-ce docker-ce-cli containerd.io
    apt-cache madison docker-ce
    # -------------------
    # Docker compose
    # -------------------
    curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    
    usermod -aG docker vagrant
    /usr/local/bin/docker-compose -f /docker-continuum/vs-test-drive/compose-testdrive.yml up --build
    echo "Done"
  SHELL
end
