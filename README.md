# scrapy-vagrant

Use this to play around with scrapy without mucking around with your personal system.

Requirements:
- Install [Vagrant](https://www.vagrantup.com)
- If on Windows, you may need to install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) as well

Usage:
- Navigate to the directory containing this Vagrantfile
- Start the VM (will provision/setup the VM the first time you run this): `vagrant up`
  - First-time setup will take some time, as it has to download a Vagrant box (essentially a VM disk file)
- Log into the VM: `vagrant ssh`
  - Once logged in, you can run scrapy straight from the commandline
  - Local directory containing Vagrantfile is shared with the VM at `/vagrant`
    - Changes made to `/vagrant` directory will persist across VM sessions
  - To exit the VM: `exit` or `logout`
- Stop the VM: `vagrant halt`
