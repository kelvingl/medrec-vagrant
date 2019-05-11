# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = [
  { :hostname => 'node1', :ip => '172.31.0.11', :box => 'kelvingl/medrec-peer', :ram => 1024, :cpu => 4},
  { :hostname => 'node2', :ip => '172.31.0.12', :box => 'kelvingl/medrec-peer', :ram => 1024, :cpu => 4},
  { :hostname => 'controller', :ip => '172.31.0.10', :box => 'kelvingl/medrec-peer', :ram => 512, :cpu => 1},
]
domain = '.network.local'

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.ssh.insert_key = true
      nodeconfig.vm.synced_folder ".", "/vagrant", owner: 'vagrant', mount_options: ['dmode=775,fmode=600']
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname] + domain
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.provider :virtualbox do |vb|
	      vb.name = node[:hostname]
        vb.memory = node[:ram] ? node[:ram] : 256
        vb.cpus = node[:cpu] ? node[:cpu] : 2
      end

      if node[:hostname] == 'controller'
        nodeconfig.vm.provision :ansible_local do |ansible|
          ansible.playbook = 'playbook.yml'
          ansible.provisioning_path = '/vagrant/ansible'
          ansible.inventory_path = "inventory"
          ansible.config_file = 'ansible.cfg'
          ansible.limit = "all"
          ansible.verbose = true
        end
      end
    end
  end
end
