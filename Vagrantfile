Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  
  (0..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "ubuntu64/noble64"
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.60.1#{i}"
      
      node.vm.provider "virtualbox" do |vb|
        vb.name = "ansible-node#{i}"
        vb.memory = "4096"
        vb.cpus = "1"
      end
      
      node.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y python3 mariadb-client-core vim git tree
        echo 'colorscheme ron' >> ~/.vimrc
        echo 'set tabstop=2' >> ~/.vimrc
        echo 'set shiftwidth=2' >> ~/.vimrc
        echo 'set expandtab' >> ~/.vimrc

        echo 'vagrant:vagrant' | sudo chpasswd

        sudo sed -i 's/^#\\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config
        if [ -f /etc/ssh/sshd_config.d/60-cloudimg-settings.conf ]; then
          sudo sed -i 's/^PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
        fi
        sudo systemctl restart ssh
      SHELL
    end
  end
end
