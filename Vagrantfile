# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  private_ip = "{{ private_ip }}"
  cpu = ENV['VAGRANT_CPU'] || 2
  ram = ENV['VAGRANT_MEMORY'] || 1024

  config.vm.network :private_network, ip: private_ip
  config.vm.network :forwarded_port, guest: 22, host: 2321

  config.ssh.insert_key = false

  config.vm.define "graphite" do |graphite|
  end

  config.vm.provider :virtualbox do |vm, override|
    vm.customize ["modifyvm", :id, "--memory", ram]
    vm.customize ["modifyvm", :id, "--cpus", cpu]

    vm.name = "graphite"
    # To increase network speed, verified!!
    #vm.customize ["modifyvm", :id, "--nictype1", "virtio"]

    # Not checked yet
    #vm.customize ["modifyvm", :id, "--nictype2", "virtio"]

    #vm.gui = true # for debug
  end

  config.vm.provision "ansible" do |ansible|
    #ansible.verbose = 'v'
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "vagrant_hosts"
    ansible.limit          = "all"
    ansible.extra_vars     = {
      target: private_ip,
      user: 'vagrant'
    }
  end
end
