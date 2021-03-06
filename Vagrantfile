# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 2.0.0"

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox"
  #config.vm.boot_timeout = "600"
  config.vm.box_check_update = "true"
  # You can find out your network interface name with 'lshw -class network'
  config.vm.network "public_network", type: "dhcp", bridge: [
    "enp3s0",
    "wlan0",
    "eth0",
    "82579LM Gigabit Network Connection",
    "82567LM-3 Gigabit Network Connection",
    "Centrino Advanced-N 6205 [Taylor Peak]"]
  config.vm.network "private_network", type: "dhcp"
  config.vm.network "private_network", type: "dhcp"
  # Enable vagrant-cachier plugin with NFS bi-directional sync for shared folders
  if Vagrant.has_plugin?("vagrant-cachier")
     config.cache.scope = :box
     config.cache.synced_folder_opts = {
       type: :nfs,
       mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
     }
    end
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.define "ubuntu", primary: true, autostart: true do |ubuntu|
    ubuntu.vm.box = "ubuntu/xenial64"
        # Fixes Apt hash sum mismatch error https://blog.packagecloud.io/eng/2016/03/21/apt-hash-sum-mismatch/
    ubuntu.vm.provision "shell", inline: "echo 'Acquire::CompressionTypes::Order:: \"gz\";' > /etc/apt/apt.conf.d/99compression-workaround"
    ubuntu.vm.provision "shell", path: "curl-me"
  end

  config.vm.define "debian", autostart: false do |debian|
    debian.vm.box = "debian/stretch64"
    debian.vm.provision "shell", path: "curl-me"
  end

  config.vm.define "fedora", autostart: false do |fedora|
    fedora.vm.box = "fedora/26-cloud-base"
    # BUG curl-me script requires 'lsb_release' which is not part of Fedora core
    fedora.vm.provision "shell", inline: "dnf -y update && dnf -y install redhat-lsb-core"
    fedora.vm.provision "shell", path: "curl-me"
  end

  config.vm.define "centos", autostart: false do |centos|
    centos.vm.box = "centos/7"
    # BUG curl-me script requires 'lsb_release' program
    centos.vm.provision "shell", inline: "yum install redhat-lsb-core"
    centos.vm.provision "shell", path: "curl-me"
  end

end
