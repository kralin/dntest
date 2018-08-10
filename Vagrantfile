Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1604"
  config.vm.provider "virtualbox" do |v|
     v.customize ["modifyvm", :id, "--memory", "512"]

  end
  config.vm.define "node1" do |n1|
    n1.vm.network "private_network", ip: "192.168.5.42"
  end
  config.vm.define "node2" do |n2|
    n2.vm.network "private_network", ip: "192.168.5.44"
  end

  config.vm.synced_folder ".", "/vagrant", type: "rsync",
                          rsync__exclude: ".git/"

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.install_mode = :pip
    ansible.version = "2.6.2"
    ansible.provisioning_path = "/vagrant/ansible"
    ansible.groups = {
      "lb" => ["node1", "node2"],
      "web" => ["node1", "node2"]
    }
#    ansible.host_vars = {
#      "node1" => {"ansible_host" => "192.168.5.42"},
#      "node2" => {"ansible_host" => "192.168.5.44"}
#    }
  end
end