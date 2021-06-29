# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

required_plugins = [ "vagrant-hostsupdater" ]
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin};vagrant #{ARGV.join(" ")}" unless Vagrant.has_plugin? plugin || ARGV[0] == 'plugin'
end

Vagrant.configure("2") do |config|


  host = RbConfig::CONFIG['host_os']

  config.vm.network "private_network", type: "dhcp"

  # For this to work you need to:  vagrant plugin install vagrant-vbguest --plugin-version 0.21
  config.vm.synced_folder ".", "/vagrant", type: "nfs"

  private_network_addr = '172.28.128.'

  config.vm.define "db" do |db|
    vm_role = 'db'
    db.vm.provider :virtualbox do |vb|
      vb.name = vm_role + "-Box"
      vb.memory = 1024
      vb.customize ["modifyvm", :id, "--description", "DB VM used to demonstrate Infrastructure as code principles via Vagrant and configure the vm with ansible."]
    end
    db.vm.network :private_network, ip: "#{private_network_addr}10"
    db.vm.box = vm_role + "-server"
    db.vm.box = "centos/7"
    db.ssh.pty = true
    db.ssh.insert_key = false
    db.vm.hostname = vm_role + "-Box"
  end

  config.vm.define "app" do |app|
    vm_role = 'app'
    app.vm.provider :virtualbox do |vb|
      vb.name = "#{vm_role}-Box"
      vb.memory = 1024
      vb.customize ["modifyvm", :id, "--description", "APP VM used to demonstrate Infrastructure as code principles via Vagrant and configure the vm with ansible."]
    end
    app.vm.network :private_network, ip: "#{private_network_addr}11"
    app.vm.network "forwarded_port", guest: 80, host: 8080
    app.hostsupdater.aliases = ["development.local"]
    app.vm.box = "#{vm_role}-server"
    app.vm.box = "centos/7"
    app.ssh.pty = true
    app.ssh.insert_key = false
    app.vm.hostname =  "#{vm_role}-Box"
    app.vm.provision "shell", path: "provision.sh" #, privileged: false
  end

  # config.vm.synced_folder "config-nginx", "/home/ubuntu/"

  # config.vm.provision :file, source: 'provision-jenkins.sh', destination: '~/provision-jenkins.sh'


  config.vm.post_up_message = "Welcome! This is the vagrant virtual environment.
  Use the command 'vagrant ssh' to access your the VM."

  # custom configuration for my ubuntu virtual machine
  # config.vm.provider "virtualbox" do |v|
  #   v.memory = 512
  # end


  #config.vm.provision "shell", path: "provision.sh"
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  # config.vm.box = "base"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
