# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "continuum-demo-vm"

  # Continuum, Windows Dell app listening on 8080, TODO turn off
  config.vm.network "forwarded_port", guest: 8080, host: 7080
  # Continuum Msghub
  config.vm.network "forwarded_port", guest: 8083, host: 8083
  # Gitlab
  config.vm.network "forwarded_port", guest: 8081, host: 8081
  # Jenkins
  config.vm.network "forwarded_port", guest: 8082, host: 8082
  # JIRA
  config.vm.network "forwarded_port", guest: 8084, host: 8084
  
  config.vm.network "public_network"
  config.vm.synced_folder "~/docker-continuum", "/docker-continuum"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "5120"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
  
  config.vm.provision "bootstrap", type: "shell", inline: <<-SHELL
    echo "Running bootstrap..."
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
    apt-get install -y docker-ce docker-ce-cli containerd.io python-pip
    apt-cache madison docker-ce
    # -------------------
    # Docker Compose
    # -------------------
    curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    
    usermod -aG docker vagrant
    echo "Finished bootstrap"
  SHELL

  config.vm.provision "demo", type: "shell", inline: <<-SHELL
    echo "Running testdrive..."
    /usr/bin/docker network create proxy
    /usr/bin/docker network create backend
    /usr/local/bin/docker-compose -f /docker-continuum/vs-demo/docker-compose.yml up --build
    echo "Finished testdrive"
  SHELL
end
