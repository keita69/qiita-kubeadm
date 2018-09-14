# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  (0..3).each do |i|

    if i == 0 then
      vm_name = "provisioner"
    elsif i == 1 then
      vm_name = "master"
    else
      vm_name = "node#{i-1}"
    end

    config.vm.define vm_name do |s|
      s.vm.box = "centos/7"
      s.vm.hostname = vm_name

      # K8Sクラスタ・ネットワーク (HostOnly)
      private_ip = "172.16.20.#{i+10}"
      s.vm.network "private_network", ip: private_ip

      s.vm.provider "virtualbox" do |v|
        v.gui = false        
        if i == 0 then
          v.cpus = 1
          v.memory = 1024
        elsif i == 1 then
          v.cpus = 1
          v.memory = 2048
        else
          v.cpus = 1
          v.memory = 2024
        end
      end

      # provision
      s.vm.provision "shell", path: "provision_init_all.sh", :privileged => true

    end
  end
  
  config.vm.define "provisioner" do |p|
    # provision
    p.vm.provision "shell", path: "provision_init_provisioner.sh", :privileged => true
  end
end
