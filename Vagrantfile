# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :nginx => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "nginx",
        :net => [
           ["192.168.11.150",  2, "255.255.255.0", "inet"],
        ]
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|
   
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      
      box.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
        config.vm.synced_folder "./", "/vagrant"
       end

      boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end

      box.vm.provision "shell", inline: <<-SHELL
        sudo mkdir -p ~root/.ssh
        sudo cp ~vagrant/.ssh/auth* ~root/.ssh
        sudo sed -i 's/#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        sudo systemctl restart sshd
        sudo apt update -y
        sudo apt install ansible -y
      SHELL
    end
  end
end