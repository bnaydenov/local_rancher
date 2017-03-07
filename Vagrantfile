# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

MASTER_MEMORY=2048
AGENT_MEMORY=2048

RANCH_MASTER_ADDRESS="172.19.8.100"
# Boxes are configured at 101, 102 and so on
RANCH_SUBNET="172.19.8"


RANCHER_SERVER_DOMAIN_NAME="ranch-svr.local"
RANCHER_CLIENT_DEF1="ranch-k8s1.local"
RANCHER_CLIENT_DEF2="ranch-k8s2.local"


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	config.vm.define "ranch_server" do |rserver|
		rserver.vm.box = "ubuntu/xenial64"
		rserver.vm.network "private_network", ip: "#{RANCH_MASTER_ADDRESS}"	
		rserver.vm.hostname = "#{RANCHER_SERVER_DOMAIN_NAME}"
		rserver.vm.provider :virtualbox do |vba|
			vba.customize ["modifyvm", :id, "--memory", MASTER_MEMORY]
			vba.customize ["modifyvm", :id, "--cpus", 2]
		end
		rserver.vm.provision "shell", inline: "sudo curl https://releases.rancher.com/install-docker/1.13.sh | sh"
		rserver.vm.provision "shell", inline: "sudo docker run -d --restart=always -p 8080:8080 rancher/server"		
	end

	config.vm.define "ranch_c_k8s1" do |rclient|
		rclient.vm.box = "ubuntu/xenial64"
		rclient.vm.network "private_network", ip: "#{RANCH_SUBNET}.101"
		rclient.vm.hostname = "#{RANCHER_CLIENT_DEF1}"
		rclient.vm.provider :virtualbox do |vba|
			vba.customize ["modifyvm", :id, "--memory", AGENT_MEMORY]
			vba.customize ["modifyvm", :id, "--cpus", 2]
		end
		# K8S works with slightly older version of Docker hence downgrading
		rclient.vm.provision "shell", inline: "sudo curl https://releases.rancher.com/install-docker/1.13.sh | sh"
	end

	config.vm.define "ranch_c_k8s2" do |rclient|
		rclient.vm.box = "ubuntu/xenial64"
		rclient.vm.network "private_network", ip: "#{RANCH_SUBNET}.102"
		rclient.vm.hostname = "#{RANCHER_CLIENT_DEF2}"
		rclient.vm.provider :virtualbox do |vba|
			vba.customize ["modifyvm", :id, "--memory", AGENT_MEMORY]
		end
		# K8S works with slightly older version of Docker hence downgrading
		rclient.vm.provision "shell", inline: "sudo curl https://releases.rancher.com/install-docker/1.13.sh | sh"
	end

end
