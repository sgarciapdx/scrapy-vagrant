# scrapy-vagrant

Use this to play around with scrapy without mucking around with your personal system.

What this does:
- Creates a Vagrant box with a Python dev environment for working on scrapy
- Sets up your scrapy fork on the VM in editable mode (meaning that any changes made to the codebase should be reflected when executing scrapy)
- Enables SSH agent forwarding on the VM, so you can use your SSH keys for interacting with GitHub and others from within the VM (if you have SSH agent forwarding set up on your system and change the remote repo's URL to SSH in `.git/config`)

Requirements for setup:
- Install [Vagrant](https://www.vagrantup.com)
- Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) if not already installed
- Install the Vagrant-Vbguest plugin: `vagrant plugin install vagrant-vbguest`
- Clone this repo
- Clone your scrapy fork into the directory containing your clone of this repo

Usage:
- Navigate to the directory containing this Vagrantfile
- Start the VM (will provision/setup the VM the first time you run this): `vagrant up`
  - First-time setup will take some time, as it has to download a Vagrant box (essentially a VM disk file). This typically takes ~30 minutes on a decent connection, but may take longer. 
  - You may see an error about Window System drivers when the provisioning process sets up Guest Additions. Don't worry about it (it's only pertinent for GUI stuff, which we won't be using). 
- Log into the VM: `vagrant ssh`
  - Once logged in, you can run scrapy straight from the commandline
  - Local directory containing Vagrantfile is shared with the VM at `/vagrant`
    - Changes made to `/vagrant` directory will persist across VM sessions
  - To exit the VM: `exit` or `logout`
- Stop the VM: `vagrant halt`
- If you encounter some issue with the setup proocess and need to start over again: `vagrant destroy`
  - Note: this won't touch your `scrapy` directory
- If you didn't clone your scrapy fork before setting up your VM, you'll have to reload your VM (it's a fairly quick process compared to destroy/up). From the scrapy-vagrant directory:
  - `vagrant reload --provision`
