# -*- mode: ruby -*-
# vi: set ft=ruby :

$num_instances = 2

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  config.ssh.insert_key = false # to use the same key between machines
  config.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa" # for ssh & ansible inventory
  config.vm.provision "shell", inline: <<-SHELL
    # Trust your enterprise certificates
    if [ -d "/vagrant/ca-trust" ]; then
      cp /vagrant/ca-trust/* /etc/pki/ca-trust/source/anchors
      update-ca-trust
    fi
  SHELL

  (1..$num_instances).each do |i|
    config.vm.define vm_name = "node#{i}" do |config|
      config.vm.hostname = "node#{i}"
      config.vm.network :private_network, ip: "172.17.177.#{i+20}" # ansible inventory
    end
  end

  config.vm.define "controller" do |config|
    config.vm.hostname = "controller"
    config.vm.network "private_network", ip: "172.17.177.11" # ansible inventory

    config.vm.provision :ansible_local do |ansible|
      ansible.playbook       = "provision/playbook.yml"
      ansible.inventory_path = "provision/inventory"
      ansible.config_file    = "provision/ansible.cfg"
      ansible.verbose        = true
      ansible.install        = true
      ansible.limit          = "all" # or only "nodes" group, etc.
    end
  end

end
