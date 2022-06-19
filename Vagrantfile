# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms = [
    { :name => "debian-jessie", :box => "debian/jessie64" },
    { :name => "debian-stretch", :box => "debian/stretch64" }
  ]

  vms.each do |opts|
    config.vm.define opts[:name] do |m|
      second_drive = sprintf('.vagrant/disk_data_%s.vdi', opts[:name])
      third_drive = sprintf('.vagrant/disk_data2_%s.vdi', opts[:name])
      m.vm.provider "virtualbox" do |v|
        v.cpus = 1
        v.memory = 256
        v.customize ['createhd', '--filename', second_drive, '--size', 1024]
        v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', second_drive]
        v.customize ['createhd', '--filename', third_drive, '--size', 1024]
        v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', third_drive]
      end

      m.vm.box = opts[:box]
      m.vm.network "private_network", type: "dhcp"
      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.verbose = 'vv'
        ansible.sudo = true
      end
    end
  end
end
