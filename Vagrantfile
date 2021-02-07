# -*- mode: ruby -*-
# vim: set ft=ruby :

$ssh_auth = <<SCRIPT
ssh-keygen -t rsa -f /root/.ssh/id_rsa -q -P ""
ssh-keygen -t rsa -f /home/vagrant/.ssh/id_rsa -q -P ""
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd
SCRIPT

Vagrant.configure(2) do |config|

  config.vm.define "wordpress" do |subconfig|
    subconfig.vm.box = "centos/7"
    subconfig.vm.hostname="wordpress"
    subconfig.vm.network "forwarded_port", guest: 8080, host: 8181
    subconfig.vm.network "forwarded_port", guest: 443, host: 8080
    subconfig.vm.network :private_network, ip: "192.168.0.13"
    subconfig.vm.provision "shell", inline: $ssh_auth
    # subconfig.vm.provision "ansible" do |ansible|
    #   ansible.playbook = "wordpress.yaml"
    # end
	  config.ssh.forward_agent = true
    subconfig.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = "1"
    end
  end

  config.vm.define "mysql" do |subconfig|
    subconfig.vm.box = "centos/7"
    subconfig.vm.hostname="mysql"
    subconfig.vm.network :private_network, ip: "192.168.0.14"
    subconfig.vm.provision "shell", inline: $ssh_auth
    # subconfig.vm.provision "ansible" do |ansible|
    #   ansible.playbook = "wordpress.yaml"
    # end
	  config.ssh.forward_agent = true
    subconfig.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = "1"
    end
  end

end
