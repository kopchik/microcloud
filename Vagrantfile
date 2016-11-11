# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF-8"
MYVMS = ["node1"] #, "node2"]

Vagrant.configure("2") do |config|
  MYVMS.each do |i|
    config.vm.define "#{i}" do |node|
      name = i
      node.vm.box = "ubuntu/xenial64"
      node.vm.hostname = name
      node.vm.box_check_update = false

      node.vm.synced_folder ".", "/microcloud"
      node.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.memory = "2048"
      end

      node.vm.provision "shell", inline: <<-SHELL
        # update machine and install python for ansible
        apt update
        apt upgrade -y
        apt install -y python-minimal

        # install latest docker
        apt install -y apt-transport-https ca-certificates
        apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
        echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" > /etc/apt/sources.list.d/docker.list
        apt update
        apt-cache policy docker-engine
        apt install -y linux-image-extra-$(uname -r) linux-image-extra-virtual
        apt install -y docker-engine
        service docker start
      SHELL
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "./provision/ansible/base_packages.yml"
      end
    end
  end
end
