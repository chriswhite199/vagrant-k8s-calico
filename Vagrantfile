HOST_SUBNET="192.168.240"

Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  config.ssh.insert_key = false

  config.vm.provider "libvirt" do |lv|
    lv.memory = 2048
    lv.cpus = 1
  end

  config.vm.define "master" do |master|
    master.vm.box = "centos/7"
    master.vm.hostname = "master"
    master.vm.provider "libvirt" do |lv|
      lv.memory = 4096
      lv.cpus = 2
    end
    master.vm.network "private_network", ip: "#{HOST_SUBNET}.100"
  end

  # worker node, centos7
  config.vm.define "node7-1" do |node7|
    node7.vm.box = "centos/7"
    node7.vm.hostname = "node7-1"
    node7.vm.network "private_network", ip: "#{HOST_SUBNET}.101"
  end

  config.vm.define "node7-2" do |node7|
    node7.vm.box = "centos/7"
    node7.vm.hostname = "node7-2"
    node7.vm.network "private_network", ip: "#{HOST_SUBNET}.102"
  end

  # worker node, rocky8
  # config.vm.define "node8" do |node8|
  #   node8.vm.box = "generic/rocky8"
  #   node8.vm.hostname = "node8"
  #   node8.vm.network "private_network", ip: "#{HOST_SUBNET}.102"
  # end

  # curl nodes - allowed and denied
  config.vm.define "allowed" do |allowed|
    allowed.vm.box = "generic/rocky8"
    allowed.vm.hostname = "allowed"

    allowed.vm.network "private_network", ip: "#{HOST_SUBNET}.200"
  end

  # curl nodes - allowed and denied
  config.vm.define "denied" do |denied|
    denied.vm.box = "generic/rocky8"
    denied.vm.hostname = "denied"
    denied.vm.network "private_network", ip: "#{HOST_SUBNET}.201"
  end

  # provision playbooks
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    # just create inventory and gather facts, post vagrant up we can run 'ansible-playbook provision.yml'
    ansible.groups = {
      "control" => ["master"],
      "worker" => ["node7-1", "node7-2"],
      "k8s" => ["master", "node7-1", "node7-2"]
    }
    ansible.playbook = "setup.yml"
  end
end
