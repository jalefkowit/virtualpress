# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.network :private_network, ip: "192.168.50.50"
  config.vm.provision "ansible_local" do |ansible|
    ansible.install = "true"
    ansible.install_mode = "default"
    ansible.verbose = "v"
    ansible.playbook = "setup.yml"
    ansible.inventory_path = "virtualpress-inventory"
    ansible.limit = "all"
  end
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
  config.vm.synced_folder ".", "/vagrant", owner:"www-data", group:"www-data", mount_options:["dmode=775", "fmode=775"]
end
