# -*- mode: ruby -*-
# vim: set ft=ruby :
# home = ENV['HOME']
home = '/otus/disks'

MACHINES = {
  :otuslinux => {
        :box_name => "centos/7",
        :ip_addr => '192.168.11.101',
	:disks => {
		:sata1 => {
			:dfile => home + '/sata1.vdi',
			:size => 250,
			:port => 1
		},
		:sata2 => {
			:dfile => home + '/sata2.vdi',
			:size => 250, # Megabytes
			:port => 2
		},
		:sata3 => {
			:dfile => home + '/sata3.vdi',
			:size => 250,
			:port => 3
		},
		:sata4 => {
			:dfile => home + '/sata4.vdi',
			:size => 250, # Megabytes
			:port => 4
		},
		:sata5 => {
			:dfile => home + '/sata5.vdi',
			:size => 250,
			:port => 5
			},
		:sata6 => {
			:dfile => home + '/sata6.vdi',
			:size => 250,
			:port => 6
		},
		:sata7 => {
			:dfile => home + '/sata7.vdi',
			:size => 250, # Megabytes
			:port => 7
		},
		:sata8 => {
			:dfile => home + '/sata8.vdi',
			:size => 250,
			:port => 8
		},
		:sata9 => {
			:dfile => home + '/sata9.vdi',
			:size => 250, # Megabytes
			:port => 9
		},
		:sata10 => {
			:dfile => home + '/sata10.vdi',
			:size => 250,
			:port => 10
		}
	}
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider :virtualbox do |vb|
            	  vb.customize ["modifyvm", :id, "--memory", "256"]
                  needsController = false
		  boxconfig[:disks].each do |dname, dconf|
			  unless File.exist?(dconf[:dfile])
				vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
                                needsController =  true
                          end

		  end
                  if needsController == true
                     vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
                     boxconfig[:disks].each do |dname, dconf|
                         vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
                     end
                  end
          end

      box.vm.provision "shell", inline: <<-SHELL
	      mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
	      yum install -y mdadm smartmontools hdparm gdisk
  	  SHELL

      end
  end
end

