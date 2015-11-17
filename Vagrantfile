# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "scrapy-vm"
 
  # We can use this later for pulling from the team repo if everybody is
  # set up with ssh forwarding.
  config.ssh.forward_agent = true 
  
  config.vm.provider "virtualbox" do |vb|
    # We need at least this much for setup
    vb.memory = "1024"
    
    # Sets up internet connectivity for vagrant
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # Install the necessary packages, switch to our shared directory, check if
  # our project directory exists, and clone it if it doesn't.
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y build-essential libssl-dev libffi-dev python-dev python3-dev libxml2-dev libxslt1-dev git
    sudo apt-get install -y python-pip python3-pip
    cd /vagrant
    if [ ! -d scrapy ]; then
      git clone https://github.com/PDX-Capstone-Team-C/scrapy.git
    else
      echo "scrapy project directory already exists, not cloning!"
    fi
    cd scrapy
    pip install -e .
  SHELL
end
