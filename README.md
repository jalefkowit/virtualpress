# VirtualPress: Easy WordPress Development Environments

This bundle of files automates the process of automatically creating and provisioning local virtual machines with a complete, running instance of WordPress. Such things are useful for local development and testing things like plugins and themes on a "clean" WordPress install.

## Prerequisites

To make use of these files, you'll need to have the following prerequisites installed on your workstation:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](http://www.vagrantup.com/)

Any Vagrant-based VM needs to start with a Vagrant "box" file -- a canned image of a base system that Vagrant can use as the starting point for further customization. **By default, these scripts use the official Vagrant image distributed by Ubuntu of the 64-bit version of Ubuntu 16.04 LTS, "Xenial Xerus," so you do not need to download or install anything further if you are satisfied with that version.** As of this writing (December 13, 2017) that file is [distributed here](https://app.vagrantup.com/ubuntu/boxes/xenial64).

If you want to build your VM from a different version or distro, check [Vagrant Cloud](https://app.vagrantup.com/boxes/search) to find a box you wish to use. If you wish to use a box other than the default, just install the box (if it's not hosted at Vagrant Cloud) and update the Vagrantfile to point to the particular box you wish to use.

Note that these scripts are designed for use with Ubuntu, so they make use of the apt packaging manager and other conventions Debian-derived distributions share (filesystem locations, configuration file structure, etc.). **This means they will only work properly out of the box with Debian Linux or Debian-derived distributions such as Ubuntu.** If you use Red Hat/CentOS/Fedora, or another distribution that uses a different package manager, consult the Ansible documentation for instructions on how to modify these scripts to replace the Debian-based commands commands to those for your particular package manager and distro.

## What It Does

Together with the prerequisites listed above, the scripts contained herein will let you create a new VM with a simple `vagrant up` that:

* Is configured with a static IP address on your local LAN (default 192.168.50.50) so you don't need to constantly be looking up new DHCP-assigned addresses each time it restarts
* Has a complete MySQL 5 setup (client and server) installed
* Has a complete Apache 2 setup installed (with mod_rewrite)
* Has a complete PHP7 setup (both PHP-FPM and CLI versions) installed, with the following modules:
    * php7.0
    * php7.0-cli
    * php7.0-curl
    * php7.0-gd
    * php-imagick
    * php7.0-mysql
    * php7.0-xmlrpc
    * php7.0-xml
	* php7.0-mbstring
    * php-xdebug (ready for remote debugging on port 10000; use the IDE key "virtualpress")
* Installs the latest version of the WordPress software in /vagrant, so you can work with local files via your favorite editor/IDE; sets up symlink to it at /var/www/wordpress`so Apache can find it
* Has a MySQL database (name: "wordpress") and database user (name: "wordpress") for WordPress to make use of
* Has a configuration file in `/root/.my.cnf` to allow the root user to log into MySQL as root without needing to enter a username or password
* Has an Apache virtual host configured and enabled to serve WordPress
* Has [WP-CLI](http://wp-cli.org/) installed at /usr/local/bin/wp

So, once the box is provisioned, all you need to do is go to 192.168.50.50 in your browser to begin the famous WordPress 5-minute install.

## Getting Started

To begin, create an empty directory and clone the files in this repository into it.

Then just cd into that directory and run the command `vagrant up`, and your VM should bootstrap itself into existence, ready to work with. When you're done working, run `vagrant halt` and your VM will safely shut itself down, freeing up your workstation's memory while saving any changes you've made. To shell into your VM while it's running, use the command `vagrant ssh`. If you want to blow the VM away completely and start again from a clean sheet of paper, `vagrant destroy` will do that.

[More information on working with Vagrant VMs, including how to shell into a running VM and shut it down safely, is available in the Vagrant documentation.](https://www.vagrantup.com/intro/getting-started/index.html)

## Customization/Configuration

**If you use the default box, no configuration should be required to get up and running.** However, in the file named `Vagrantfile`, you *may* wish to change the following:

* **If you want to use an IP address other than 192.168.50.50,** replace that address with the one you wish to use in `Vagrantfile` on line 9.

If you wish to modify or extend the basic logic that provisions the system -- add new packages, say -- all that logic is available to you. Everything is rolled up into an [Ansible "playbook"](http://docs.ansible.com/ansible/latest/playbooks_intro.html) located in the file setup.yml, with configuration for individual components in the system broken out into [Ansible "roles"](http://docs.ansible.com/ansible/latest/playbooks_reuse_roles.html) for modularity. For more information on how to work with Ansible, consult [the Ansible documentation.](http://docs.ansible.com/ansible/latest/index.html)

## License

These files are copyright 2017, Jason Lefkowitz.

VirtualPress is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

VirtualPress is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <http://www.gnu.org/licenses/>.
