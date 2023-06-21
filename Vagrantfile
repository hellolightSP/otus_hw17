# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :server => {
        :box_name => "CentOS-7-x86_64-Vagrant-2004_01",
        :box_version => "0",
                :cpus => 2,
                :memory => 1024,
        :ip_addr => '192.168.56.160',
     :disks => {
        :sata1 => {
            :dfile => './sata1.vdi',
            :size => 2048,
            :port => 1
        }
       }
    },

  :client => {
        :box_name => "CentOS-7-x86_64-Vagrant-2004_01",
        :box_version => "0",
                :cpus => 2,
                :memory => 1024,
        :ip_addr => '192.168.56.150',
      }

}

  Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
      config.vm.define boxname do |box|
        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s
        box.vm.provider :virtualbox do |vb|
                     vb.memory = boxconfig[:memory]
                     vb.cpus = boxconfig[:cpus]

case boxname.to_s
when "server"
        needsController = false
        boxconfig[:disks].each do |dname, dconf|
              unless File.exist?(dconf[:dfile])
              vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
         needsController =  true
       end
        end
            if needsController == true
                vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
                boxconfig[:disks].each do |dname, dconf|
                vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
        end
       end

  end
end
            config.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/backup/install.yaml"
            ansible.inventory_path = "ansible/backup/hosts"
                        ansible.host_key_checking = "false"
                        ansible.limit = "all"
        end
     end
  end
end