Vagrant.configure("2") do |config|
  config.vm.define "ubuntu1" do |ubuntu1|
    ubuntu1.vm.box = "ubuntu/focal64"
    ubuntu1.vm.hostname = "ubuntu1"
    ubuntu1.vm.network "private_network", ip: "192.168.33.10"
    ubuntu1.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y ansible
  SHELL
    #ubuntu1.vm.network "private_network", ip: "2001:db8:1234::10"
    #ubuntu1.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "2001:db8:1234::10"
  end

  config.vm.define "ubuntu2" do |ubuntu2|
    ubuntu2.vm.box = "ubuntu/focal64"
    ubuntu2.vm.hostname = "ubuntu2"
    ubuntu2.vm.network "forwarded_port", guest: 22, host: 2223
    ubuntu2.vm.network "private_network", ip: "192.168.33.11"
    #ubuntu2.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "2001:db8:1234::11"
    ubuntu2.vm.provision "shell", inline: "echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config && systemctl restart sshd.service"
  end

  config.vm.define "ubuntu3" do |ubuntu3|
    ubuntu3.vm.box = "ubuntu/focal64"
    ubuntu3.vm.hostname = "ubuntu3"
    ubuntu3.vm.network "private_network", ip: "192.168.33.12"
    #ubuntu3.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "2a01:4f9:3a:3c0b::100"
    ubuntu3.vm.network "forwarded_port", guest: 22, host: 2224
    ubuntu3.vm.provision "shell", inline: "sudo useradd -m ubuntu && echo 'ubuntu:password' | chpasswd && usermod -aG sudo ubuntu"
  end

  config.vm.define "jumphost" do |jumphost|
    jumphost.vm.box = "ubuntu/focal64"
    jumphost.vm.hostname = "jumphost"
    jumphost.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.33.13"
    jumphost.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = "1"
  end
end
  config.vm.define "centos7" do |centos7|
    centos7.vm.box = "centos/7"
    centos7.vm.hostname = "centos7"
    centos7.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.1.2"
    centos7.vm.network "forwarded_port", guest: 22, host: 2225
    centos7.vm.provision "shell", inline: "sudo yum install -y epel-release && sudo yum install -y sshpass && sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config && sudo systemctl restart sshd.service"
    centos7.ssh.forward_agent = true
  end
end