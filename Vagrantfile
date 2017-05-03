required = ["vagrant-docker-compose", "vagrant-cachier", "vagrant-hostmanager"]

restart = false
for req in required
    unless Vagrant.has_plugin?(req)
      system("vagrant plugin install #{req}")
      puts "Dependencies #{req} installed."
      restart = true
    end
end

if restart then
    puts "Dependencies installed, please relaunch vagrant up."
    exit
end


Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.provision :docker
  config.vm.provision :docker_compose


  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define 'default' do |node|
      node.vm.hostname = 'footify-box'
      node.vm.network "private_network", type: "dhcp"

      node.vm.network "forwarded_port", guest: 3000, host: 3000 #Login service
      node.vm.network "forwarded_port", guest: 22, host: 9022 #SSH

      node.hostmanager.aliases = %w(login.footify.guedj.tech api.footify.guedj.tech)
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
  end
end
