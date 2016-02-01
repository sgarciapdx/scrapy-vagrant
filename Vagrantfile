# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  
  # Define the scrapy VM as the primary machine
  config.vm.define "scrapy-vm", primary: true do |scrapy|
    scrapy.vm.box = "ubuntu/trusty64"
    scrapy.vm.hostname = "scrapy-vm"
    scrapy.ssh.forward_agent = true 
    scrapy.vm.network "private_network", ip: "10.10.10.9"
    scrapy.vm.provider "virtualbox" do |vb|
      # We need at least this much for setup
      vb.memory = "1024"
      # Sets up internet connectivity for vagrant
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end

    # Install the necessary packages, switch to our shared directory, check if
    # our project directory exists, and clone it if it doesn't.
    scrapy.vm.provision "shell", inline: <<-SHELL
      sudo add-apt-repository "deb http://us.archive.ubuntu.com/ubuntu/ precise universe"
      sudo apt-get update
      sudo apt-get install -y build-essential libssl-dev libffi-dev python-dev python3-dev libxml2-dev libxslt1-dev zlib1g-dev libjpeg-dev git
      sudo apt-get install -y python-pip python3-pip
      sudo apt-get install -y python-leveldb python-xdelta3
      pip install -U tox
      cd /vagrant
      if [ -d scrapy ]; then
        cd scrapy
        pip install -e .
      else
        echo "/vagrant/scrapy project directory not found, not installing!"
        echo "You can set up your fork later, see README.md for details."
      fi
    SHELL
  end

  # Don't start up the web VM by default
  config.vm.define "web", autostart: false do |web|
    web.vm.box = "ubuntu/trusty64"
    web.vm.hostname = "web-vm"
    web.ssh.forward_agent = true
    web.vm.network "private_network", ip: "10.10.10.10"
    web.vm.provider "virtualbox" do |vb|
      # Keeping it small for now
      vb.memory = "512"
      # Sets up internet connectivity for vagrant
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end

    # Install apache, remove the old document root, create and
    # symlink the new document root
    web.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      if ! [ -L /var/www ]; then
        sudo rm -rf /var/www
        if ! [ -d /vagrant/web ]; then
          mkdir -p /vagrant/web/html
        fi
        ln -fs /vagrant/web /var/www
      fi
    SHELL
  end
end
