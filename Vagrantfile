Vagrant.configure("2") do |config|

  ###################################################################################
  config.vm.define "woodpecker-agent" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.6.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 4096
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "woodpecker-agent"

    # disable shared folders
    node.vm.synced_folder ".", "/vagrant", disabled: true

  end # config.vm.define

  ###################################################################################
  config.vm.define "woodpecker-server" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.6.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 8196
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "woodpecker-server"

    # disable shared folders
    node.vm.synced_folder ".", "/vagrant", disabled: true

    # Ansible
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.limit = "all"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/group_vars/all/woodpecker_oauth_credentials.yml"
      trigger.run = {inline: "rm -vf ansible/group_vars/all/woodpecker_oauth_credentials.yml"}
    end # node.trigger.after

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/group_vars/all/woodpecker_agent_secret.yml"
      trigger.run = {inline: "rm -vf ansible/group_vars/all/woodpecker_agent_secret.yml"}
    end # node.trigger.after

  end # config.vm.define

end # Vagrant.configure
