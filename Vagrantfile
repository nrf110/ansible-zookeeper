# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

Vagrant.configure("2") do |config|
  config.vm.box = 'ubuntu/xenial64'
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = false
  config.hostmanager.ignore_private_ip = false

  node_count = 3
  machines = (1..node_count).map { |idx| "zookeeper#{idx}" }
  groups = {
    'zookeeper' => machines
  }

  machines.each_with_index do |name,idx|
    config.vm.define name do |instance|
      instance.vm.network :private_network, ip: "192.168.52.#{2 + idx}"
      instance.vm.hostname = name

      instance.vm.provision "shell" do |s|
        s.inline = "sudo apt-get install -y python"
      end

      if name == machines.last
        instance.vm.provision "ansible" do |ansible|
          ansible.groups = groups
          ansible.limit = 'all'
          ansible.playbook = 'test.yml'
          ansible.verbose = 'vv'
          ansible.sudo = true
        end
      end
    end
  end
end
