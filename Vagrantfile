NETWORK = "192.168.56"
NETMASK = "255.255.255.0"

def setup_vm(vm, name, ip, memory=1024, cpus=1)
    vm.vm.hostname = name
    vm.vm.network :private_network, ip: ip, netmask: NETMASK
    vm.vm.provider :virtualbox do |v|
        v.cpus = cpus
        v.memory = memory
        v.name = name
    end
end

Vagrant.configure("2") do |config|
    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.check_guest_additions = false
        vb.linked_clone = false
    end

    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.ssh.insert_key = false
    config.vm.box = "centos/stream9"

    config.vm.define "pgha" do |vm|
        setup_vm(vm, "pgha", "#{NETWORK}.100", 1024, 1)
    end

    (1..3).each do |n|
        config.vm.define "pgnode#{n}" do |vm|
            setup_vm(vm, "pgnode#{n}", "#{NETWORK}.#{n + 6}", 1512, 1)
            vm.vm.disk :disk, size: "25GB", name: "pgnode#{n}_data"
        end
    end
end
