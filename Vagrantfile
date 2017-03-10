# lustre-zfs-test
#
# Vagrantfile - manifest file for Vagrant to provision virtual machines
# Author: Ilari Korhonen, KTH Royal Institute of Technology
#
# Copyright (C) 2016-2017 KTH Royal Institute of Technology. All rights reserved.
# See LICENSE file for more information.

# JSON array for the Vagrants to be configured
boxes = [
  {
    :name => "lustre-target0",
    :eth1 => "192.168.56.60",
    :eth2 => "10.13.37.60",
    :mem => "1024",
    :cpu => "1"
  },
  {
    :name => "lustre-client0",
    :eth1 => "192.168.56.61",
    :eth2 => "10.13.37.61",
    :mem => "1024",
    :cpu => "1"
  }
]

Vagrant.configure(2) do |config|

  # base box for the system image
  config.vm.box = "centos/7"

  # disable shared folders
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]

      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end

      # virtualbox host only network for host connectivity
      config.vm.network :private_network, ip: opts[:eth1]

      # virtualbox private network for the HA network
      config.vm.network :private_network, ip: opts[:eth2], virtualbox__intnet: true
    end
  end
end
