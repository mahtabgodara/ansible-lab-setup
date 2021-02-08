# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "centos/7"

  public_key = File.read("./id_rsa.pub")
  private_key = File.read("./id_rsa")

  config.vm.provision "shell", inline: <<-SHELL 

    sudo yum update -y
    sudo yum install -y epel-release
    sudo yum install -y ansible
    sudo yum install -y git
    
    sudo useradd ansible
    echo ansible | passwd ansible --stdin
    usermod -aG wheel ansible
    
    sudo -u ansible bash
    mkdir /home/ansible/.ssh
    chmod 700 /home/ansible/.ssh
    echo '#{private_key}' > /home/ansible/.ssh/id_rsa
    chmod 600 /home/ansible/.ssh/id_rsa
    echo '#{public_key}' >> /home/ansible/.ssh/authorized_keys
    chmod -R 600 /home/ansible/.ssh/authorized_keys
    echo 'Host *' >> /home/ansible/.ssh/config
    echo 'StrictHostKeyChecking no' >> /home/ansible/.ssh/config
    echo 'UserKnownHostsFile /dev/null' >> /home/ansible/.ssh/config
    chmod -R 600 /home/ansible/.ssh/config
    sudo chown -R ansible:ansible /home/ansible/

    sudo sh -c 'echo "192.168.61.10  control\n192.168.61.11  db\n192.168.61.12 www\n192.168.61.13  lb" >> /etc/hosts'
    
  SHELL

  # VAGRANT_COMMAND = ARGV[0]
  # if VAGRANT_COMMAND == "ssh"
  #   config.ssh.username = 'ansible'
  #   config.ssh.password = 'ansible'    
  #   config.ssh.private_key_path = "./id_rsa"
  # end

  config.vm.define "control" do |control|
    control.vm.network :private_network, ip: "192.168.61.10"
  end

  config.vm.define "db" do |db|
    db.vm.network :private_network, ip: "192.168.61.11"
  end
  
  config.vm.define "www" do |www|
    www.vm.network :private_network, ip: "192.168.61.12"
  end

  config.vm.define "lb" do |lb|
    lb.vm.network :private_network, ip: "192.168.61.13"
  end

end

