Vagrant.configure("2") do |config|
  # master vm
  config.vm.define "first" do |centos_jenkins_first|
	 centos_jenkins_first.vm.box="centos/7"
		centos_jenkins_first.vm.provider :virtualbox do |v|
		v.customize ["modifyvm", :id, "--memory", 10240]
		v.customize ["modifyvm", :id, "--name", "centos_jenkins_first"]
	  end
	  centos_jenkins_first.vm.network "forwarded_port", guest: 8080, host: 8080
	  centos_jenkins_first.vm.network "private_network",
		type:"dhcp"
	 centos_jenkins_first.vm.provision "shell", inline: <<-SHELL
	   sudo yum install wget vim yum-utils -y
	   yum-config-manager --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
       yum-config-manager --enable docker-ce-edge
	   yum install -y docker-ce
	   systemctl enable docker
	   systemctl start docker
       usermod -aG docker vagrant
	   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	   sudo chmod +x /usr/local/bin/docker-compose
	   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
	 SHELL
   end

config.vm.define "second" do |centos_second|
	 centos_second.vm.box="centos/7"
		centos_second.vm.provider :virtualbox do |v|
		v.customize ["modifyvm", :id, "--memory", 10240]
		v.customize ["modifyvm", :id, "--name", "centos_second"]
	  end
	  centos_second.vm.network "forwarded_port", guest: 8081, host: 8081
	  centos_second.vm.network "private_network",
		type:"dhcp"
	 centos_second.vm.provision "shell", inline: <<-SHELL 
	   sudo yum install wget vim yum-utils -y
	   yum-config-manager --add-repo \
	   https://download.docker.com/linux/centos/docker-ce.repo
	   yum-config-manager --enable docker-ce-edge
	   yum install -y docker-ce
       systemctl enable docker
	   systemctl start docker
	   usermod -aG docker vagrant
	   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	   sudo chmod +x /usr/local/bin/docker-compose
	   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
	 SHELL
   end
end
