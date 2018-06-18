unless Vagrant.has_plugin?("vagrant-hostsupdater")
  raise 'vagrant-hostsupdater is not installed!  Run "vagrant plugin install vagrant-hostsupdater"'
end
unless Vagrant.has_plugin?("vagrant-vbguest")
  raise 'vagrant-vbguest is not installed!  Run "vagrant plugin install vagrant-vbguest"'
end

Vagrant.configure(2) do |config|

    config.vm.define "nagios.yourdomain.local" do |nagios|
        nagios.vm.box = "serotonin/debian9"
        nagios.vm.network "private_network", ip: "192.168.123.200"
        nagios.hostsupdater.aliases = ["nagios.yourdomain.local"]

        nagios.vm.provider "virtualbox" do | v |
            v.memory = 1024
            v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
            v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        end

        nagios.vm.provision "ansible" do |ansible|
            #ansible.verbose = "vvvv"
            #ansible.tags = ['nagios']
            ansible.playbook = "provisioning/playbook.yml"
            ansible.inventory_path = "provisioning/inventory-local"
            ansible.host_key_checking = false
            ansible.become = true
            ansible.become_user = "root"
            ansible.limit = 'nagios.yourdomain.local'
            ansible.extra_vars = {
            ansible_ssh_args: '-o ForwardAgent=yes'
            }
        end
    end
end