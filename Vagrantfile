HOST_SUBNET="192.168.240"

Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky8"
  config.vm.box_check_update = false
  config.ssh.insert_key = false

  config.vm.provider "libvirt" do |lv|
    lv.memory = 2048
    lv.cpus = 1
  end

  config.vm.define "master" do |master|
    master.vm.hostname = "master"
    master.vm.provider "libvirt" do |lv|
      lv.memory = 4096
      lv.cpus = 2
    end
    master.vm.network "private_network", ip: "#{HOST_SUBNET}.100"
  end

  # worker node, centos7
  config.vm.define "worker-1" do |node7|
    node7.vm.hostname = "worker-1"
    node7.vm.network "private_network", ip: "#{HOST_SUBNET}.101"
  end

  config.vm.define "worker-2" do |node7|
    node7.vm.hostname = "worker-2"
    node7.vm.network "private_network", ip: "#{HOST_SUBNET}.102"
  end

  # provision playbooks
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    # just create inventory and gather facts, post vagrant up we can run 'ansible-playbook provision.yml'
    ansible.groups = {
      "control" => ["master"],
      "worker" => ["worker-1", "worker-2"],
      "k8s" => ["master", "worker-1", "worker-2"]
    }
    ansible.playbook = "setup.yml"
  end
end
