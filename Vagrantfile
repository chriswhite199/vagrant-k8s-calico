Vagrant.configure("2") do |config|
  config.vm.box_check_update = true
  config.vm.provider "libvirt" do |lv|
    lv.memory = 2048
    lv.cpus = 1
  end

  config.vm.define "master" do |master|
    master.vm.box = "generic/rocky8"
    master.vm.hostname = "master"
    master.vm.provider "libvirt" do |lv|
      lv.memory = 4096
      lv.cpus = 2
    end
  end

  # worker node, centos7
  config.vm.define "node7" do |node7|
    node7.vm.box = "centos/7"
    node7.vm.hostname = "node7"
  end

  # worker node, rocky8
  config.vm.define "node8" do |node8|
    node8.vm.box = "generic/rocky8"
    node8.vm.hostname = "node8"
  end

  # provision playbooks
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    # just create inventory and gather facts, post vagrant up we can run 'ansible-playbook provision.yml'
    ansible.groups = {
      "control" => ["master"],
      "worker" => ["node7", "node8"]
    }
    ansible.playbook = "setup.yml"
  end
end
