MACHINES = {
	:ansible => {
		:box_name => "almalinux/8",
#		 :box_version => "2004.01",
		:ip_addr => '192.168.1.2',
		:script => './ansible.sh',
	},
	:server1 => {
		:box_name => "almalinux/8",
#		 :box_version => "2004.01",
		:ip_addr => '192.168.1.3',
		:script => './server.sh',
	},
	:server2 => {
		:box_name => "almalinux/8",
#		 :box_version => "2004.01",
		:ip_addr => '192.168.1.4',
		:script => './server.sh',
	},
}
 

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
	config.vm.define boxname do |box|
		config.vm.box = boxconfig[:box_name]
		  config.vm.box_check_update = false
#		  box.vm.box_version = boxconfig[:box_version]
		box.vm.host_name = boxname.to_s
		box.vm.network "private_network", ip: boxconfig[:ip_addr], netmask: "255.255.255.0", virtualbox__intnet: "ansible_net"
		box.vm.provider :virtualbox do |vb|
		  vb.customize ["modifyvm", :id, "--memory", "512"]
		end
		box.vm.provision "shell", inline: <<-SHELL
# 		  sudo yum -y update
#		  sudo yum install -y nfs-utils
#		  sudo yum install -y epel-release
#		  sudo yum install -y sshfs
		  echo -en "192.168.1.2 ansible\n192.168.1.3 server1\n192.168.1.4 server2\n\n" | sudo tee -a /etc/hosts
		  SHELL
		box.vm.provision "shell", path: boxconfig[:script]
	end
  end
end