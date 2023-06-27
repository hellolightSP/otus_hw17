disk_controller = 'IDE' # MacOS. This setting is OS dependent. Details https://github.com/hashicorp/vagrant/issues/8105

MACHINES = {

  :client => {
        :box_name => "CentOS-7-x86_64-Vagrant-2004_01",
        :box_version => "0",
        :ip_addr => '192.168.56.150',
  },

  :server => {
        :box_name => "CentOS-7-x86_64-Vagrant-2004_01",
        :box_version => "0",
        :ip_addr => '192.168.56.160',

     :disks => {
        :sata2 => {
            :dfile => './sata57.vdi',
            :size => 2048,
            :port => 1
                 }
          } 
  },

}

Vagrant.configure("2") do |config|

    config.vm.box_version = "0"
    MACHINES.each do |boxname, boxconfig|
   config.vm.synced_folder ".", "/vagrant", disabled: true
        config.vm.define boxname do |box|

            box.vm.box = boxconfig[:box_name]
            box.vm.host_name = boxname.to_s

            #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

            box.vm.network "private_network", ip: boxconfig[:ip_addr]

            box.vm.provider :virtualbox do |vb|
                    vb.customize ["modifyvm", :id, "--memory", "2048"]
                    vb.customize ["modifyvm", :id, "--cpus", "2"] 

case boxname.to_s
 when "server"
        needsController = false
        boxconfig[:disks].each do |dname, dconf|
              unless File.exist?(dconf[:dfile])
              vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
         needsController =  true
      end
    end
  end
            if needsController == true
                vb.customize ["storagectl", :id, "--name", "IDE57", "--add", "sata" ]
                boxconfig[:disks].each do |dname, dconf|
                vb.customize ['storageattach', :id,  '--storagectl', 'IDE', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
         end
       end
     end

    config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/backup/install.yaml"
    ansible.limit = "all"
    ansible.verbose = "true"
      end
    end
  end
end
