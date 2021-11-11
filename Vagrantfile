ENV['VAGRANT_NO_PARALLEL'] = 'yes'
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
        config.vm.provision "ansible" do |ansible1|
          ansible1.become = true
          ansible1.verbose = "v"
          ansible1.playbook = "lava-master.yaml"
          ansible1.galaxy_role_file = "requirements.yml"
          ansible1.inventory_path = "inventories/lava-servers.ini"
        end
      end
      if opts[:name] == "lava-workers"
        config.vm.provision "ansible" do |ansible2|
          ansible2.become = true
          ansible2.verbose = "v"
          ansible2.limit = "all"
          ansible2.playbook = "lava-workers.yaml"
          ansible2.galaxy_role_file = "requirements.yml"
          ansible2.inventory_path = "inventories/lava-servers.ini"
        end
      end

    end
  end
end
