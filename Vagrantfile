Vagrant.require_version ">= 2.0.0"

boxes = [
    {
        :name => "ansible",
        :eth1 => "192.168.201.10",
        :mem => "1024",
        :cpu => "1"
    },
    {
        :name => "node1",
        :eth1 => "192.168.201.11",
        :mem => "1024",
        :cpu => "1"
    },
    {
        :name => "node2",
        :eth1 => "192.168.201.12",
        :mem => "1024",
        :cpu => "1"
#    },
#    {
#        :name => "node3",
#        :eth1 => "192.168.201.13",
#        :mem => "1024",
#        :cpu => "1"
    }

]

Vagrant.configure(2) do |config|
  config.vm.box = "generic/ubuntu2204"

  boxes.each do |opts|
      config.vm.define opts[:name] do |config|
        config.vm.hostname = opts[:name]

        config.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--memory", opts[:mem]]
          v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
        end
        config.vm.network :private_network, ip: opts[:eth1]
      end
  end
  config.vm.provision "shell", inline: <<-SHELL
     sudo apt update
     sudo apt install vim wget tree -y
   SHELL
end
