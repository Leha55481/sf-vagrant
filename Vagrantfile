Vagrant.configure("2") do |config|
  config.vm.box = "failfish/precise64"
  config.vm.box_version = "1.0.4"
  config.vm.hostname = "pg84"
  config.vm.network "private_network", ip: "192.168.56.84"

  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"                # use KVM (default)
    libvirt.memory = 1024                 # allocate 1GB RAM
    libvirt.cpus = 1                      # single CPU
    libvirt.disk_bus = "virtio"           # better performance
    libvirt.nic_model_type = "virtio"     # faster network
  end

  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    sudo sh -c 'echo "deb http://old-releases.ubuntu.com/ubuntu/ precise main universe" > /etc/apt/sources.list'
    sudo apt-get update
    sudo apt-get install -y wget ca-certificates debconf-utils

    echo "postgresql-common postgresql-common/obsolete-major error" | sudo debconf-set-selections

    sudo apt-get install -y postgresql-8.4

    sudo dpkg --configure -a

    sudo service postgresql status
  SHELL
end
