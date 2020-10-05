# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  (1..2).each do |i|
    config.vm.define vm_name = "node#{i}" do |machine|
      machine.vm.hostname = "node#{i}"
      machine.vm.network :private_network, ip: "172.17.177.#{i+20}"
    end
  end

  config.vm.define "controller" do |machine|
    machine.vm.hostname = "controller"
    machine.vm.network "private_network", ip: "172.17.177.11"

    machine.vm.provision :ansible_local do |ansible|
      ansible.playbook       = "provision/playbook.yml"
      ansible.inventory_path = "provision/inventory"
      ansible.config_file    = "provision/ansible.cfg"
      ansible.verbose        = true
      ansible.install        = true
      ansible.limit          = "all" # or only "nodes" group, etc.
    end
  end

end
