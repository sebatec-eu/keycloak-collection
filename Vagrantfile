# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "sebatec-eu/debian-stable"
  # config.vm.box = "debian/testing64"

  config.vm.provider "libvirt" do |vb, override|
    vb.driver = "kvm"
    vb.loader = "/usr/share/ovmf/OVMF.fd"
    vb.memory = 4*1024
    vb.cpus = 2
  end

  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 8081, host: 8081

  config.vm.synced_folder './', '/vagrant', type: 'sshfs'

  config.vm.provision "shell", inline: <<-SCRIPT
    apt-get update
    DEBIAN_FRONTEND=noninteractive apt-get upgrade -y
    DEBIAN_FRONTEND=noninteractive apt-get install -y man
  SCRIPT

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/vagrant.yaml"
    ansible.config_file = './ansible.cfg'
  end
end
