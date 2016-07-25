VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.username='vagrant'
  config.vm.box='ubuntu/trusty64'
  config.vm.hostname='open-development-environment-java'
  config.vm.network "private_network", type: "dhcp"

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpus", 4]
    v.customize ["modifyvm", :id, "--memory", 4096]
    v.customize ["modifyvm", :id, "--name", config.vm.hostname]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    v.customize ["modifyvm", :id, "--vram", "128"] # 10 MB is the minimum to enable Virtualbox seamless mode

    # Display the VirtualBox GUI
    v.gui = true
  end

  # Copy Ansible provisioning playbooks
  #config.vm.provision "file", source: "provisioning/ansible", destination: "/tmp/provisioning/ansible"

  # Install Ansible
  config.vm.provision "ansible_local" do |ansible|
    ansible.install = true
    ansible.install_mode = :pip
    ansible.playbook = "check-ansible-installation.yml"
    ansible.provisioning_path= "/tmp/provisioning/ansible"
    ansible.version = "2.1.0.0"
  end

  # Install the required Ansible roles
  config.vm.provision "shell", path: "provisioning/scripts/install_ansible_roles.sh"

  # Run Ansible from the Vagrant VM
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "open-development-environment-java.yml"
    ansible.provisioning_path= "/tmp/provisioning/ansible"
  end

  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.synced_folder "provisioning/ansible", "/tmp/provisioning/ansible"
end