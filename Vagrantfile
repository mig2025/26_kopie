# -*- mode: ruby -*-
# vi: set ft=ruby :
MACHINES = {
  :client => {
    :box_name => "ubuntu/jammy64",
    :box_version => "2204",
    :ip_addr => "192.168.56.150",
  },
  :backup => {
    :box_name => "ubuntu/jammy64",
    :box_version => "2204",
    :ip_addr => "192.168.56.160"
  }
}
Vagrant.configure("2") do |config|
  MACHINES.each do |boxname,boxconfig|
    config.vm.define boxname do |bb|
      bb.vm.box = boxconfig[:box_name]
      bb.vm.box_check_update = false
      bb.vm.host_name = boxname.to_s
      bb.vm.network "private_network",ip: boxconfig[:ip_addr]
      bb.vm.provider :virtualbox do |vb|
        vb.memory = "4096"
        vb.cpus = 2
      end
      if(boxname.to_s == "client" || boxname.to_s == "backup")
        (1..1).each do |i|
          bb.vm.disk :disk, size: "2048MB", name: "disk-#{i}"
        end
      end
      if(boxname.to_s == "client")
        bb.vm.provision "ansible" do |ansible|
          ansible.verbose = "v"
          ansible.playbook = "client.yml"
        end
      end
      if(boxname.to_s == "backup")
        bb.vm.provision "ansible" do |ansible|
          ansible.verbose = "v"
          ansible.playbook = "server.yml"
        end
      end
    end
  end
end
