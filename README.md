# scrapy-vagrant

Use this to play around with scrapy without mucking around with your personal system.

Requirements:
- Install [Vagrant](https://www.vagrantup.com) 

Usage:
- Navigate to the directory containing this Vagrantfile
- Start the VM (will provision/setup the VM the first time you run this): `vagrant up`
- Log into the VM: `vagrant ssh`
  - Once logged in, you can run scrapy straight from the commandline
  - Local directory containing Vagrantfile is shared with the VM at `/vagrant`
    - Changes made to `/vagrant` directory will persist across VM sessions
  - To exit the VM: `exit` or `logout`
- Stop the VM: `vagrant halt`
