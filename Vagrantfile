Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_check_update = false
  config.vm.network "private_network", type: "dhcp" 
  config.vm.provider "vmware_desktop" do |v|
    v.memory = 1024 
    v.cpus   = 2  
    v.gui    = false 
  end
end
