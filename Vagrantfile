#Vagrantfile

Vagrant.configure("2") do |config|
    servers=[
        {
            :hostname => "Node01",
            :box => "bento/ubuntu-18.04", 
            :ip => "172.16.1.51",
            :ssh_port => '2201'
        }, 
        {
            :hostname => "Node02",
            :box => "bento/ubuntu-18.04", 
            :ip => "172.16.1.52", 
            :ssh_port => '2202'
        }
        
    ]

    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network :private_network, ip: machine[:ip]
            if machine[:mysql] == true then
                node.vm.network "forwarded_port", guest: 3306, host: 3306
            end
            node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
            node.vm.provider :virtualbox do |vb|
                vb.customize ["modifyvm", :id, "--memory", 512]
                vb.customize ["modifyvm", :id, "--cpus", 1]
            end
            
        end
    end
    
  # Run Ansible from the Vagrant VM
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/playbook.yaml"
    ansible.inventory_path= 'inventories/inventory'
  end

end
