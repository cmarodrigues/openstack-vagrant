
# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = {
    'controller'  => [1, 200],
    'compute' => [1, 201],
    'swift' => [1, 210],
    'ceilometer' => [1, 250],
    'cinder' => [1, 211],
}

Vagrant.configure("2") do |config|
    config.vm.box = "precise64"
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"

    #Default is 2200..something, but port 2200 is used by forescout NAC agent.
    config.vm.usable_port_range= 2800..2900 

    nodes.each do |prefix, (count, ip_start)|
        count.times do |i|
            hostname = "%s" % [prefix, (i+1)]
            config.vm.define "#{hostname}" do |box|
                box.vm.hostname = "#{hostname}.book"
                box.vm.network :private_network, ip: "172.16.0.#{ip_start+i}", :netmask => "255.255.0.0"
                box.vm.network :private_network, ip: "10.10.0.#{ip_start+i}", :netmask => "255.255.0.0" 
				box.vm.network :private_network, ip: "192.168.100.#{ip_start+i}", :netmask => "255.255.255.0" 
				# Otherwise using VirtualBox
				box.vm.provider :virtualbox do |vbox|
					# Defaults
					vbox.customize ["modifyvm", :id, "--memory", 2048]
					vbox.customize ["modifyvm", :id, "--cpus", 1]
                    if prefix == "compute"
                        vbox.customize ["modifyvm", :id, "--memory", 3172]
                        vbox.customize ["modifyvm", :id, "--cpus", 2]
                    end
                    if prefix == "cinder"
                        vbox.customize ["modifyvm", :id, "--memory", 1024]
                        vbox.customize ["modifyvm", :id, "--cpus", 1]
                    end
                    if prefix == "swift"
                        vbox.customize ["modifyvm", :id, "--memory", 1024]
                        vbox.customize ["modifyvm", :id, "--cpus", 1]
                        vbox.customize ["createhd", "--filename", 'swift_disk2.vdi', "--size", 2000 * 1024]
                        vbox.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', 'swift_disk2.vdi']
                    end
				end
			end
		end
	end
end

