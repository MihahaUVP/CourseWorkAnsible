Vagrant.require_version ">= 1.8.0"
Vagrant.configure('2') do | config |
    config.vm.define 'second' do |second|
        second.vm.box = 'Ubuntu-Vagrant'
        second.vm.hostname = 'base.lab'
        second.vm.provider 'virtualbox' do |vb|
            vb.customize ['modifyvm', :id, '--memory', '1024']
        end
	second.vm.network "private_network", ip: "192.168.99.101"
        second.vm.network "forwarded_port", guest: 80, host: 8080
    end
    config.vm.define 'server' do |server|
        server.vm.box = 'Ubuntu-Vagrant'
        server.vm.hostname = 'base.lab'
        server.vm.provider 'virtualbox' do |vb|
            vb.customize ['modifyvm', :id, '--memory', '1024']
        end
	server.vm.network "private_network", ip: "192.168.99.100"
	server.vm.provision "shell",
	    inline: "sudo apt-add-repository ppa:ansible/ansible"
	server.vm.provision "shell",
	    inline: "sudo apt-get update"
	server.vm.provision "shell",
	    inline: "yes Y | sudo apt-get install ansible"
	server.vm.provision "shell",
	    inline: "echo 'second ansible_host=192.168.99.101 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant' >> /etc/ansible/hosts"
	server.vm.provision "shell",
	    inline: "ssh-keyscan 192.168.99.101 >> ~/.ssh/known_hosts"
	server.vm.provision "file", source: "./playbook.yml", destination: "playbooks/playbook.yml"
	server.vm.provision "shell",
	    inline: "ansible-playbook playbooks/playbook.yml -l second -u vagrant"
    end  
end