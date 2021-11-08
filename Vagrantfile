# ansible-galaxy collection install community.general

Vagrant.configure("2") do |config|
  config.vm.define "lava-master" do |lavamaster|
    lavamaster.vm.box = "generic/debian10"
    lavamaster.ssh.insert_key = false
#    lavamaster.vm.network :private_network, ip: "192.168.121.86"
    lavamaster.vm.hostname = "lava-master.box"
#    lavamaster.vm.network "forwarded_port", guest: 3000, host: 4000
    lavamaster.vm.provider :libvirt do |domain|
      domain.memory = 2048
      domain.cpus = 2
      domain.nested = true
    end
    config.vm.provision "ansible" do |ansible|
      ansible.become = true
      ansible.verbose = "v"
      ansible.playbook = "lava-master.yaml"
#     ansible.galaxy_role_file = "requirements.yml"
#      ansible.extra_vars = "secrets.yml"
    end
  end
end
