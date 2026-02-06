Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  
  (0..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "generic/ubuntu2204"
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.56.1#{i}"
      
      node.vm.provider "virtualbox" do |vb|
        vb.name = "ansible-node#{i}"
        vb.memory = "1024"
        vb.cpus = "1"
      end
      
      node.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y python3 vim git tree
      SHELL
    end
  end
end


