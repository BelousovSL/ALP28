ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'
Vagrant.configure(2) do |config|
  

   config.vm.define "mastermy" do |mastermy| 
      mastermy.vm.box = 'bento/ubuntu-24.04'
      mastermy.vm.hostname = 'mastermy'
      mastermy.vm.network :forwarded_port, host: 2801, guest: 22
      mastermy.vm.network :private_network, ip: '192.168.12.150', netmask: '255.255.255.0', virtualbox__intnet: 'my_net2', adapter: 2
      mastermy.vm.provider "virtualbox" do |vbx|
         vbx.memory = "4048"
         vbx.cpus = "2"
         vbx.customize ["modifyvm", :id, '--audio', 'none']
      end

         mastermy.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible_mastermy/provision.yml"
            ansible.inventory_path = "ansible_mastermy/hosts"
            ansible.host_key_checking = "false"
            ansible.limit = "all"
        end
   end

   config.vm.define "slavemy" do |slavemy| 
      slavemy.vm.box = 'bento/ubuntu-24.04'
      slavemy.vm.hostname = 'slavemy'
      slavemy.vm.network :forwarded_port, host: 2802, guest: 22
      slavemy.vm.network :private_network, ip: '192.168.12.151', netmask: '255.255.255.0', virtualbox__intnet: 'my_net2', adapter: 2
      slavemy.vm.provider "virtualbox" do |vbx|
         vbx.memory = "4048"
         vbx.cpus = "2"
         vbx.customize ["modifyvm", :id, '--audio', 'none']
      end

         slavemy.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible_slavemy/provision.yml"
            ansible.inventory_path = "ansible_slavemy/hosts"
            ansible.host_key_checking = "false"
            ansible.limit = "all"
        end
   end

end
