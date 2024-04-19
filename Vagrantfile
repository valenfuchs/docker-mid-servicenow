# Create a minimal Ansible box
Vagrant.require_version ">= 1.8.1"
Vagrant.configure(2) do |config|


    # Cache downloaded packages for this box
    if Vagrant.has_plugin?("vagrant-cachier")
        config.cache.scope = :box
    end


    # Patch for Vagrant <1.8.2: https://github.com/mitchellh/vagrant/issues/6793
    # Make sure it comes before any other provisioners
    config.vm.provision "shell" do |s|
        s.inline = '[[ ! -f $1 ]] || grep -F -q "$2" $1 || sed -i "/__main__/a \\    $2" $1'
        s.args = ['/usr/bin/ansible-galaxy', "if sys.argv == ['/usr/bin/ansible-galaxy', '--help']: sys.argv.insert(1, 'info')"]
    end


    # Configure the base box
    config.vm.define "ubuntu" do |ubuntu|
        ubuntu.vm.box = "ubuntu/jammy64"
        ubuntu.vm.box_version = "20231215.0.0"
        ubuntu.vm.synced_folder "ansible-docker/", "/ansible-docker/", type: "rsync"
        ubuntu.vm.network "forwarded_port", guest: 8000, host: 8000
    end


    # Configure the VirtualBox parameters
    config.vm.provider "virtualbox" do |vb|
        vb.name = "ansible-docker" 
        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected", "--memory", "2048" ]
    end


    # Configure the box with Ansible
    config.vm.provision "ansible_local" do |ansible_local|
        #ansible.install = false
        #ansible.compatibility_mode = "2.0"
        ansible_local.playbook = "ansible-vagrant/playbook.yml"
    end
    config.vm.provision "ansible" do |ansible_remote|
        ansible_remote.galaxy_role_file = "./requirements.yml"
        ansible_remote.playbook = "playbooks/docker.yml"
        ansible_remote.groups = {
          "docker" => ["ubuntu"],
          
        }
        ansible_remote.verbose = "v"
      end

end