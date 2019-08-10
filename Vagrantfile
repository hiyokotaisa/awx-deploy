# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
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
    sudo yum -y install epel-release
    sudo yum -y install python-pip git ansible docker
    sudo pip install docker docker-compose
    sudo systemctl start docker
    sudo setenforce 0
    sudo git clone https://github.com/ansible/awx.git
    sudo ansible-playbook -i /home/vagrant/awx/installer/inventory /home/vagrant/awx/installer/install.yml

  SHELL

end
