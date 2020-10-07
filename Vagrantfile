# -*- mode: ruby -*-
# vim: set ft=ruby :
home = ENV['HOME']
ENV["LC_ALL"] = "en_US.UTF-8"

MACHINES = {
  :master => {
    :ip_addr => '192.168.11.136',
  },
  :slave => {
    :ip_addr => '192.168.11.137',
  },
}

Vagrant.configure("2") do |config|
  config.vbguest.no_install = true
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.box = 'centos/7'

  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.network "private_network", ip: boxconfig[:ip_addr]
      box.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
      end
    end
  end
  config.vm.define 'slave' do |slave|
    slave.vm.provision :ansible do |ansible|
      ansible.limit = "all"
      ansible.playbook = "playbook.yml"
      ansible.inventory_path = "hosts"
    end
  end
end
