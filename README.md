#Alpine Vagrant Box setup for VirtualBox

##Box preparation

Install the base image `alpine-standard-3.7.0-x86_64.iso` in VirtualBox

Uncomment all repositories in `/etc/apk/repositories` and install the VirtualBox guest additions
https://wiki.alpinelinux.org/wiki/VirtualBox_guest_additions

Uncomment the line `UseDNS no` in `/etc/ssh/sshd_config` to avoid ssh connexion delay afterward.

Install the shadow and sudo packages `# apk add sudo` `$ sudo apk add shadow`

Add the `vagrant` user and upload the unsecure public key https://www.vagrantup.com/docs/boxes/base.html#quot-vagrant-quot-user

Package the box and add it to your Vagrant runtime environment

`$ vagrant package --base alpine-node --output alpine.box`

`$ vagrant box add alpine alpine.box`

Install the vagrant-alpine plugin to avoid this kind of issue `change_host_name on the detect guest OS 'linux', but the guest does not support that capability`, thanks to https://github.com/maier/vagrant-alpine : `$ vagrant plugin install vagrant-alpine`

Now you're ready to bootstrap some VM(s) : `$ vagrant init` `$ vagrant up`

##Note
When packaging it's mandatory to put the base image NIC in NAT network otherwise vagrant will not being able to log into the VM with the vagrant user through a local port forwarding.
After the VM has been spawned, the additional NIC are eventually added and configured in the desired network following the vagrant file statements `config.vm.network "public_network", bridge: "wlp2s0",...`
