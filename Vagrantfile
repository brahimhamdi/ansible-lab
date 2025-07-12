# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Configuration commune à toutes les machines
  config.vm.box_check_update = false
  
  # Configuration de la VM Ansible (AlmaLinux 9)
  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "generic/alma9"
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.56.200"
    
    ansible.vm.provider "virtualbox" do |vb|
      vb.name = "ansible-controller"
      vb.memory = "1024"
      vb.cpus = "1"
    end
    
    # Provisionnement pour installer Ansible
    ansible.vm.provision "shell", inline: <<-SHELL
      sudo dnf install -y epel-release
      sudo dnf install -y ansible
#      sudo sed -i 's/^#host_key_checking = False/host_key_checking = False/' /etc/ansible/ansible.cfg
#      echo -e "[nodes]\nnode1 ansible_host=192.168.56.11\nnode2 ansible_host=192.168.56.12" | sudo tee /etc/ansible/hosts
    SHELL
  end

  # Configuration des nœuds dans une boucle
  (1..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "generic/ubuntu2204"
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.56.20#{i}"
      
      node.vm.provider "virtualbox" do |vb|
        vb.name = "ansible-node#{i}"
        vb.memory = "1024"
        vb.cpus = "1"
      end
      
      # Provisionnement de base pour Ubuntu
      node.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update -qq
        sudo apt-get install -y python3
      SHELL
    end
  end
end
