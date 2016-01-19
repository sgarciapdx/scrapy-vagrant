# scrapy-vagrant

Use this to play around with scrapy without mucking around with your personal system.

What this does:
- Creates a Vagrant box with a Python dev environment for working on scrapy (called `scrapy-vm`)
  - Default IP for this box is 10.10.10.9
- Creates a Vagrant box running Apache for serving test sites (called `web`)
  - Default IP for this box is 10.10.10.10
  - This serves up any pages found in this directory's `web/html` subdirectory
  - To view pages served from this VM, point your browser to `http://10.10.10.10`
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
- Start the VMs you want to use (will provision/setup each VM the first time you run this): `vagrant up <boxname>`
  - Running `vagrant up` without specifying a VM name will start up `scrapy-vm` but not `web`.
  - First-time setup will take some time, as it has to download a Vagrant box (essentially a VM disk file). This typically takes ~30 minutes on a decent connection, but may take longer. 
  - You may see an error about Window System drivers when the provisioning process sets up Guest Additions. Don't worry about it (it's only pertinent for GUI stuff, which we won't be using). 
- Log into a specific VM: `vagrant ssh <boxname>`
  - Once logged into `scrapy-vm`, you can run scrapy straight from the commandline
  - Local directory containing Vagrantfile is shared with the VM at `/vagrant`
    - `web` uses `/vagrant/web/html` as its document root for now
    - Changes made to anything in the `/vagrant` directory will persist across VM sessions
  - To exit the VM: `exit` or `logout`
- Stop all VMs: `vagrant halt`
- Stop a specific VM: `vagrant halt <boxname>`
- If you encounter some issue with the setup proocess and need to start over again: `vagrant destroy <boxname>`
  - Note: this won't touch your `scrapy` or `web` directories
- If you didn't clone your scrapy fork before setting up your VM, you'll have to reload your VM (it's a fairly quick process compared to destroy/up). From the scrapy-vagrant directory:
  - `vagrant reload --provision <boxname>` or `vagrant up --provision <boxname>`

Important:
- When using anything that requires LevelDB, make sure LevelDB is not using any shared directories. Failing to do so will result in an IO error due to a years-old issue with Virtualbox and how its shared directories work. Instead, make sure LevelDB is using a directory local to the VM. For example, `/home/vagrant/httpcache` would work (assuming the directory exists). 
