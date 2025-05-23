Vagrant.configure("2") do |config|
  # Provision common.sh to all nodes
  config.vm.provision "file", source: "scripts/common.sh", destination: "/tmp/common.sh"

  # Initial host configuration
  config.vm.provision "shell", inline: <<-SHELL
      apt-get update -y
      echo "10.0.0.10  master-node" >> /etc/hosts
      echo "10.0.0.11  worker-node01" >> /etc/hosts
      echo "10.0.0.12  worker-node02" >> /etc/hosts
  SHELL

  config.vm.define "master" do |master|
    master.vm.box = "bento/ubuntu-22.04"
    master.vm.hostname = "master-node"
    master.vm.network "private_network", ip: "10.0.0.10"
    master.vm.provider "virtualbox" do |vb|
        vb.memory = 4048
        vb.cpus = 2
    end

    # Provision master.sh only to master node
    master.vm.provision "file", source: "scripts/master.sh", destination: "/tmp/master.sh"

    # Execute scripts on master node
    master.vm.provision "shell", inline: <<-SHELL
        chmod +x /tmp/common.sh
        chmod +x /tmp/master.sh
        /tmp/common.sh
        /tmp/master.sh
    SHELL
  end

  (1..2).each do |i|
    config.vm.define "node0#{i}" do |node|
      node.vm.box = "bento/ubuntu-22.04"
      node.vm.hostname = "worker-node0#{i}"
      node.vm.network "private_network", ip: "10.0.0.1#{i}"
      node.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 1
      end

      # Execute common.sh on worker nodes
      node.vm.provision "shell", inline: <<-SHELL
          chmod +x /tmp/common.sh
          /tmp/common.sh
      SHELL
    end
  end
end