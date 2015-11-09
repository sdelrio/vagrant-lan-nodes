# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.6.0"

NUM_INSTANCES = ENV['NUM_INSTANCES'] || 8
NODE_MEM= ENV['NODE_MEM'] || 1024
NODE_CPUS = ENV['NODE_CPUS'] || 1

# Bridged adapter
BRIDGE_IF = ENV['BRIDGE_IF'] || 'Intel(R) Ethernet Connection (2) I218-V'

$script = <<SCRIPT
if [ -f /vagrant/id_rsa.pub ]; do
  cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
done
SCRIPT

BASE_IP_NET = ENV['BASE_IP_NET'] || "192.168.0"
BASE_IP_SUBNET = ENV['BASE_IP_SUBNET'] || 100
BASE_NODE_NAME = ENV['BASE_NODE_NAME'] ||"nodo-"

Vagrant.configure(2) do |config|

  # Box to use
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false

  (1..NUM_INSTANCES.to_i).each do |i|

    hostname = "#{ BASE_NODE_NAME }%02d" % (i)

    config.vm.define hostname do |node|
      bridge_ip = "#{ BASE_IP_NET }.#{ BASE_IP_SUBNET + i}"
      node.vm.network "public_network", ip: bridge_ip , :bridge => BRIDGE_IF
      node.vm.provision "shell", inline: $script
      node.vm.provider "virtualbox" do |node|
        node.name   = hostname
        node.memory = NODE_MEM
        node.cpus   = NODE_CPUS
      end
      puts "#{ hostname } >" + " CPUs: #{ NODE_CPUS }," + " RAM: #{ NODE_MEM } MB," + " Bridged IP: #{ bridge_ip }"
    end
  end

end
