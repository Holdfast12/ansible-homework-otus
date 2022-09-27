MACHINES = {
	:ansible => {
		:box_name => "almalinux/8",
		:ip_addr => '192.168.1.2',
		:script => './ansible.sh',
	},
	:server1 => {
		:box_name => "almalinux/8",
		:ip_addr => '192.168.1.3',
		:script => './server.sh',
	},
	:server2 => {
		:box_name => "almalinux/8",
		:ip_addr => '192.168.1.4',
		:script => './server.sh',
	},
}
 

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
	config.vm.define boxname do |box|
		config.vm.box = boxconfig[:box_name]
		  config.vm.box_check_update = false
		box.vm.host_name = boxname.to_s
		box.vm.network "private_network", ip: boxconfig[:ip_addr], netmask: "255.255.255.0", virtualbox__intnet: "ansible_net"
		box.vm.provider :virtualbox do |vb|
		  vb.customize ["modifyvm", :id, "--memory", "512"]
		end
		box.vm.provision "shell", inline: <<-SHELL
		  echo -en "192.168.1.2 ansible\n192.168.1.3 server1\n192.168.1.4 server2\n\n" | sudo tee -a /etc/hosts
		  cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
		  cp /vagrant/id_rsa /home/vagrant/.ssh
		  cp /vagrant/id_rsa.pub /home/vagrant/.ssh
		  sudo chown 700 /home/vagrant/.ssh/id_rsa.pub /home/vagrant/.ssh/id_rsa
		  echo -en 'ANSIBLE_CONFIG="/vagrant/ansible/ansible.cfg"\n' | sudo tee -a /etc/environment
		  for env in $( cat /etc/environment ); do export $(echo $env | sed -e 's/"//g'); done
		  SHELL
		box.vm.provision "shell", path: boxconfig[:script]
	end
  end
end

# запуск плейбука
# ansible-playbook /vagrant/ansible/provision/playbook.yml