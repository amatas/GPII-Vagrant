# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "chef/centos-7.0"

  config.vm.network "forwarded_port", guest: 8082, host: 8888

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end

  config.vm.synced_folder ".", "/opt/GPII"

$script = <<SCRIPT
yum -y update
yum -y install epel-release
yum -y install docker ansible python-docker-py
yum -y clean all
systemctl enable docker
systemctl start docker
ssh-keygen -f /root/.ssh/id_rsa -t rsa -N ''
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
echo -e "Host localhost\n    StrictHostKeyChecking no" > /root/.ssh/config
cd /opt/GPII
ansible-playbook -i localhost gpii-playbook.yml
SCRIPT

  config.vm.provision "shell", inline: $script
end


