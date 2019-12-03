# -*- mode: ruby -*-
# vi: set ft=ruby :
vm_box         = ENV['BOX'] ? ENV['BOX'] : 'centos/7'

# The hostname of the Vagrant VM

vm_hostname    = ENV['HOSTNAME'] ? ENV['HOSTNAME'] : 'vagrantpuppet'

# The IP address to assign to the VM
# Default: 192.168.16.20
# Examples:
# VM_IP=192.168.1.4
vm_ip       = ENV['VM_IP'] ? ENV['VM_IP'] : '192.168.16.21'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
# Minimum Vagrant Version
Vagrant.require_version ">= 2.2.0"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "#{vm_hostname}" do |puppet|
    # Global Box details
    puppet.vm.box = "#{vm_box}"
    puppet.vm.hostname = "#{vm_hostname}"

    # Box Specifications
    # VirtualBox
    puppet.vm.provider :virtualbox do |vb|
      vb.gui = false      # Change to true to launch console
      vb.name = "#{vm_hostname}"
      vb.memory = 4096
      vb.cpus = 2
    end

    # VMWare Desktop (fusion/workstation)
    ["vmware_fusion", "vmware_workstation"].each do |provider|
      puppet.vm.provider provider do |vmw, override|
        vmw.gui = false   # Change to true to launch console
        vmw.vmx["ethernet0.virtualDev"] = "vmxnet3"
        vmw.vmx["memsize"] = 4096
        vmw.vmx["numvcpus"] = 2
        # Do not overwrite pci-slot number (https://www.vagrantup.com/docs/vmware/boxes.html#making-compatible-boxes)
        config.vm.provider provider do |vmware|
          vmware.whitelist_verified = true
        end
      end
    end

    # Configure a private network
    puppet.vm.network :private_network, ip: "#{vm_ip}"
  end
end
