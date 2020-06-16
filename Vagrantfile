# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "2", "--ioapic", "on"]
  end

  # configuration for awx vm
  config.vm.define "awx1" do |tower|
    tower.vm.hostname = "awx1"
    tower.vm.network "private_network", ip: "192.168.100.100"
  end
  
  # configuration for installing required packages and run installer
  config.vm.provision "shell", inline: <<-SHELL
    # enable docker-ce repo
    sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo dnf -y install epel-release wget
    # install containerd before the docker-ce installation
    sudo wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.13-3.2.el7.x86_64.rpm
    sudo dnf localinstall -y containerd.io-1.2.13-3.2.el7.x86_64.rpm m
    sudo rm -f containerd.io-1.2.13-3.2.el7.x86_64.rpm
    sudo dnf -y install python3-pip git docker-ce ansible
    sudo pip3 install docker docker-compose
    sudo systemctl start docker
    sudo setenforce 0
    sudo git clone https://github.com/ansible/awx.git
    # install awx
    sudo ansible-playbook -i /home/vagrant/awx/installer/inventory /home/vagrant/awx/installer/install.yml -e ansible_python_interpreter='/usr/bin/python3'

  SHELL

end
