Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box = "generic/debian10"
  config.vm.provider :libvirt do |domain|
      domain.memory = 2048
      domain.cpus = 2
      domain.nested = true
  end
  boxes = [
        { :name => "lava-master", :ip => "192.168.77.10" },
        { :name => "lava-workers", :ip => "192.168.77.11" }
  ]
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]
      if opts[:name] == "lava-master"
        config.vm.provision "ansible" do |ansible|
          ansible.become = true
          ansible.verbose = "v"
          ansible.playbook = "lava-master.yaml"
          ansible.galaxy_role_file = "requirements.yml"
          ansible.inventory_path = "inventories/lava-servers.ini"
        end
      end
      if opts[:name] == "lava-workers"
        config.vm.provision "ansible" do |ansible|
          ansible.become = true
          ansible.verbose = "v"
          ansible.limit = "all"
          ansible.playbook = "lava-workers.yaml"
          ansible.galaxy_role_file = "requirements.yml"
          ansible.inventory_path = "inventories/lava-servers.ini"
        end
      end

    end
  end
end
