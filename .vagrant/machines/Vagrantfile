# to be able to configure typed triggers
#ENV['VAGRANT_EXPERIMENTAL'] = 'typed_triggers'

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end
  
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key"]
  

  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/impish64"
    ansible.vm.boot_timeout = 999
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.44.10"
    ansible.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "~/.ssh/insecure_private_key"
    ansible.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/insecure_private_key"
    $root_shell = <<-EOF
    apt-get update
    apt-get -y install git
    apt-get -y install python3-pip
    apt-get -y install ansible
    apt-get install sshpass
    chmod 0600 /home/vagrant/.ssh/insecure_private_key
    ssh-keygen -p -N "" -f /home/vagrant/.ssh/insecure_private_key
    pip install pywinrm
    pip install requests-credssp
    pip install requests-kerberos
    pip install omsdk --upgrade
    pip install isi_sdk_8_1_1
    pip install isi_sdk_9_0_0
    EOF
    $user_shell = <<-EOF
    ansible-galaxy collection install dellemc.os10
    ansible-galaxy collection install dellemc.openmanage
    ansible-galaxy collection install dellemc.powerscale
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install chocolatey.chocolatey
    ansible-galaxy collection install community.vmware
    cd /home/vagrant
    git clone git@github.com:rbagsby/ansible-role-gitlab.git
    EOF
    ansible.vm.provision "shell", inline: $root_shell
    ansible.vm.provision "shell", inline: $user_shell, privileged: false
  end

end