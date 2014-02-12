# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

$install_chef = <<SCRIPT
if ! [ -d '/opt/chef' ];
then
  curl -L https://www.opscode.com/chef/install.sh | sudo bash
fi
SCRIPT

provisioner = {
  cookbooks: "cookbooks",
  roles: "roles",
  databags: "data_bags"
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provision :shell, :inline => $install_chef

  config.vm.define "centos" do |centos|
    centos.vm.box = "Centos-6.4-x86_64"
    if Vagrant.has_plugin?("vagrant-cachier")
      centos.cache.auto_detect = true
      # If you are using VirtualBox, you might want to enable NFS for shared folders
      # centos.cache.enable_nfs  = true
    end
    centos.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = provisioner[:cookbooks]
      chef.roles_path = provisioner[:roles]
      chef.data_bags_path = provisioner[:databags]
      chef.arguments = '-l debug'
      chef.run_list = [
        "recipe[chef-solo-search]",
        "recipe[zookeeperd]",
        "recipe[zookeeperd::server]"
      ]
      chef.json = {
        
      }
    end
  end

  config.vm.provision :shell, :inline => $install_chef

  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.box = "ubuntu-12.04-x86_64"
    if Vagrant.has_plugin?("vagrant-cachier")
      ubuntu.cache.auto_detect = true
      # If you are using VirtualBox, you might want to enable NFS for shared folders
      # ubuntu.cache.enable_nfs  = true
    end
    ubuntu.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = provisioner[:cookbooks]
      chef.roles_path = provisioner[:roles]
      chef.data_bags_path = provisioner[:databags]
      chef.arguments = '-l debug'
      chef.run_list = [
        "recipe[chef-solo-search]",
        "recipe[zookeeperd::server]"
      ]
      chef.json = {

      }
    end
  end

  config.vm.provision :shell, :inline => $install_chef
 
  config.vm.define "ubuntu_cloudera" do |ubuntu_cloudera|
    ubuntu_cloudera.vm.box = "ubuntu-12.04-x86_64"
    if Vagrant.has_plugin?("vagrant-cachier")
      ubuntu_cloudera.cache.auto_detect = true
      # If you are using VirtualBox, you might want to enable NFS for shared folders
      # ubuntu.cache.enable_nfs  = true                    
    end
    ubuntu_cloudera.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = provisioner[:cookbooks]
      chef.roles_path = provisioner[:roles]
      chef.data_bags_path = provisioner[:databags]
      chef.arguments = '-l debug'
      chef.run_list = [
        "recipe[chef-solo-search]",
        "recipe[zookeeperd::server]"
      ]
      chef.json = {
        "zookeeperd" => {
          "cloudera_repo" => true
        }
      }
    end
  end

end
