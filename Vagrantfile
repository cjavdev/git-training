# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

# Checks if the current version is >= to given version.
def using_at_least(version)
  Gem::Version.create(version) <= Gem::Version.create(Vagrant::VERSION)
end

def processor_count
  case RbConfig::CONFIG['host_os']
  when /darwin9/
    `hwprefs cpu_count`.to_i
  when /darwin/
    ((`which hwprefs` != '') ? `hwprefs thread_count` : `sysctl -n hw.physicalcpu`).to_i
  when /linux/
    `cat /proc/cpuinfo | grep processor | wc -l`.to_i
  when /freebsd/
    `sysctl -n hw.ncpu`.to_i
  end
end

def total_memory
  base = 0
  case RbConfig::CONFIG['host_os']
  when /darwin/
    base = `sysctl -n hw.memsize`.to_i / 1024 / 1024
  when /linux/
    base = `free -m | grep Mem | awk '{print $2}'`.to_i
  when /freebsd/
    base = `sysctl -n hw.memsize`.to_i / 1024 / 1024
  end
  # Dedicate only half of physical memory to vm.
  base / 2
end

Vagrant.configure(2) do |config|

  # Base image is Ubuntu Trusty LTS 14.04 with Linux LTS kernel 3.14
  config.vm.box = "ubuntu/trusty64"
  # config.vm.box_url = "http://packages/virtualboxes/myvr-dev.json"

  config.vm.network "private_network", ip: "172.28.128.3"

  # Forward port 3306 to MySQL server.
  config.vm.network "forwarded_port", host: 8036, guest: 3306

  # Forward port 8000 to Django server.
  config.vm.network "forwarded_port", host: 8000, guest: 8000
  config.vm.network "forwarded_port", host: 8001, guest: 8001

  # Forward port 8888 for iPython notebook server.
  config.vm.network "forwarded_port", host: 8888, guest: 8888

  # Do not launch the VB GUI, use 50% memory and same # of CPU as host.
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = processor_count
    vb.gui = false
    vb.memory = total_memory

    # Use the KVM paravirtualization provider that is optimal for Linux
    # See: https://www.virtualbox.org/manual/ch10.html#gimproviders
    vb.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]

    # Use intel nics, they have the best performance for our setup.
    # See: https://www.virtualbox.org/manual/ch06.html#network_performance
    vb.customize ["modifyvm", :id, "--nictype1", "82540EM"]
    vb.customize ["modifyvm", :id, "--nictype2", "82540EM"]

    # Turn on ACPI, allows virtualbox to get power on/off state from the
    # machine (i.e. allows fast shutdown). Without this "on" vagrant halt
    # will take forever...zzz.
    vb.customize ["modifyvm", :id, "--acpi", "on"]

    # Takes advatage of whatever virtualization acceleration is available.
    # See: https://www.virtualbox.org/manual/ch10.html#idp46785390519504
    vb.customize ["modifyvm", :id, "--vtxvpid", "on"]
    vb.customize ["modifyvm", :id, "--largepages", "on"]
  end

  # Use uid and gid mount options instead of user/group because the user
  # will not exist for a new instance before provisioning.
  mount_options = ["uid=#{Process.uid}", "gid=#{Process.gid}"]

  # Mount users .ssh remote fab commands.
  config.vm.synced_folder "~/.ssh", "/home/ubuntu/.ssh",
                          mount_options: mount_options

  # Mount .gnupg for commands like rebuild_devdb, deploy, etc..
  config.vm.synced_folder "~/.gnupg", "/home/ubuntu/.gnupg",
                          mount_options: mount_options

  # Mount project directory as /home/ubuntu/myvr instead of /vagrant.
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder ".", "/home/ubuntu/demo",
                          nfs: true,
                          mount_options: [
                            'rsize=32768',
                            'wsize=32768'
                          ]

  # Mount salt configuration for provisioning.
  # config.vm.synced_folder "salt/roots/", "/srv/salt/",
  #                         nfs: true,
  #                         mount_options: ['rsize=8192', 'wsize=8192']
  #
  # # Make sure vagrant user can modify the salt minion config.
  # config.vm.provision :shell do |shell|
  #   shell.inline = "chgrp vagrant /etc/salt/minion && "\
  #                  "chmod 664 /etc/salt/minion "
  # end
  #
  # # Provision the VM via salt with masterless minion.
  # config.vm.provision :salt do |salt|
  #   # Bootstrap script is a noop to prevent it from running. The base vm box
  #   # already has the appropriate salt version. We do this to prevent salt
  #   # from changing from underneath us.
  #   salt.masterless = true
  #   salt.bootstrap_script = "salt/bootstrap.sh"
  #   salt.minion_config = "salt/minion"
  #   salt.run_highstate = true
  #   salt.log_level = "error"
  #   salt.verbose = true
  #
  #   pillar_data = YAML.load_file("salt/pillar/base/dev.sls")
  #   pillar_data["dev-user"] = {
  #     "uid" => Process.uid,
  #     "gid" => Process.gid
  #   }
  #   salt.pillar(pillar_data)
  # end
  #
end
