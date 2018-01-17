# -*- mode: ruby -*-
# vi: set ft=ruby :

# README
#
# Getting Started:
# 1. vagrant plugin install vagrant-hostmanager
# 2. vagrant plugin install vagrant-vbguest
# 3. vagrant up
# 4. vagrant ssh master01
#
# This should put you at the control host
#  with access, by name, to other vms
Vagrant.configure(2) do |config|
  config.hostmanager.enabled = true

  config.vm.box = "centos/7"
  config.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", "2048"]
          vb.customize ["modifyvm", :id, "--cpus", "2"]
      end
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox", disabled: false
  config.vm.define "master01", primary: true do |h|
    h.vm.network "private_network", ip: "192.168.135.11"
    h.vm.hostname = "master01"
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
  echo 'ssh key generated'
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub
echo 'Copied ssh key'
cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
h.vm.provision :shell, path: "bootstrap.sh"
  end

  config.vm.define "slave01" do |h|
    h.vm.network "private_network", ip: "192.168.135.201"
    h.vm.hostname = "slave01"
    h.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", "2048"]
          vb.customize ["modifyvm", :id, "--cpus", "2"]
      end
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
    h.vm.provision :shell, path: "bootstrap.sh"
  end

  config.vm.define "slave02" do |h|
    h.vm.network "private_network", ip: "192.168.135.211"
    h.vm.hostname = "slave02"
    h.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", "2048"]
          vb.customize ["modifyvm", :id, "--cpus", "2"]
      end
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
    h.vm.provision :shell, path: "bootstrap.sh"
  end
end
