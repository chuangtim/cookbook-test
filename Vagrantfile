# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

Vagrant.require_version '>= 1.5.0'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # config.vm.hostname = 'cookbook-test/-berkshelf'
  config.vm.provider "virtualbox" do |v|
    v.name = "SitePoint Test Vagrant"
    v.customize ["modifyvm", :id, "--memory", "512"]
  end

  config.vm.box = "work_base"
  if Vagrant.has_plugin?("vagrant-cachier")
     config.cache.auto_detect = true
     config.cache.scope = :machine
     config.omnibus.cache_packages = true
     #config.omnibus.install_url = "./chefinstall.sh"
     # config.omnibus.chef_version = "11.10.0"
  end

  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  # Assign this VM to a host-only network IP, allowing you to access it
  # via the IP. Host-only networks can talk to the host machine as well as
  # any other machines on the same network, but cannot be accessed (through this
  # network interface) by any external networks.
  # config.vm.network :private_network, type: 'dhcp'

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  # The path to the Berksfile to use with Vagrant Berkshelf
  # config.berkshelf.berksfile_path = "./Berksfile"

  # Enabling the Berkshelf plugin. To enable this globally, add this configuration
  # option to your ~/.vagrant.d/Vagrantfile file
  config.berkshelf.enabled = true
  config.vm.define "app" do |layer|

    # layer.vm.provision "shell", inline: "apt-get update"
    layer.berkshelf.enabled = true
    layer.berkshelf.berksfile_path = "Berksfile"
    layer.vm.provision "chef_solo" do |chef|  
      # chef.json = {
      #   "nodejs" => {
      #     "install_method" => "binary",
      #     "version" => "0.12.9"
      #   }
      # }
      chef.run_list = ["cookbook-test::default"]
    end

    # Forward port 80 so we can see our work
    layer.vm.network "forwarded_port", guest: 80, host: 8080
    # layer.vm.network "private_network", ip: "10.10.10.10"
  end
  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to exclusively install and copy to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  # config.vm.provision :chef_solo do |chef|
  #   chef.json = {
  #     mysql: {
  #       server_root_password: 'rootpass',
  #       server_debian_password: 'debpass',
  #       server_repl_password: 'replpass'
  #     }
  #   }

  #   chef.run_list = [
  #     'recipe[cookbook-test/::default]'
  #   ]
  # end
end
