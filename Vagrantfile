# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_API_VER = "2"
# All Vagrant configuration is done below. The "2" above
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

VM_NAME = "rabbitmq-test-server"
Vagrant.configure(VAGRANT_API_VER) do |config|
  config.vm.box = "naphatkrit/rabbitmq"
  config.vm.hostname = VM_NAME

  # Disable automatic box update checking. 
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine.
  config.vm.network "forwarded_port", guest: 5672, host: 5672
  config.vm.network "forwarded_port", guest: 15672, host: 15672

  # config.vm.network "private_network", ip: "192.168.33.11"
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM.
  config.vm.synced_folder "#{Dir.pwd}/data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    # vb.memory = "1024"
    vb.name = VM_NAME
  end
 
  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys

      echo "Enabling RabbitMQ management console"
      sudo /usr/lib/rabbitmq/bin/rabbitmq-plugins enable rabbitmq_management rabbitmq_management_agent rabbitmq_management_visualiser --offline
      sudo service rabbitmq-server restart
    SHELL
  end
end
