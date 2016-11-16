# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF-8"
MYVMS = [
  {
    name:"srv",
    ip: "10.100.198.200",
    playbook: "./provision/ansible/base_packages.yml"
  },{
    name: "node1",
    ip: "10.100.198.201",
    playbook: "./provision/ansible/base_packages.yml"
  }
]

Vagrant.configure("2") do |config|
  MYVMS.each do |cfg|
    config.vm.define "#{cfg[:name]}" do |node|
      name = cfg[:name]
      node.vm.network "private_network", ip: cfg[:ip]
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

        # docker compose
        curl -L "https://github.com/docker/compose/releases/download/1.8.1/docker-compose-$(uname -s)-$(uname -m)" > /usr/local/bin/docker-compose
        chmod +x /usr/local/bin/docker-compose
      SHELL
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = cfg[:playbook]
      end
    end
  end
end
