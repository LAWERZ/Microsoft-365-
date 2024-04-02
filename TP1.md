**TP1**

**Vagrantfile 1**

```
Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky9"

end
```

**Vagrantfile 2 Stockage + RAM**

```
Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky9"

  # Configuration réseau
  config.vm.network "private_network", ip: "10.1.1.11"

  # Configuration du nom d'hôte
  config.vm.hostname = "ezconf.tp1.efrei"

  # Configuration des ressources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.customize ["createhd", "--filename", "ezconf.tp1.efrei.vdi", "--size", 20 * 1024] # 20GB
  end
end
```

**Vagrantfile 3 Script**
```
Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky9"

  # Configuration réseau
  config.vm.network "private_network", ip: "10.1.1.11"

  # Configuration du nom d'hôte
  config.vm.hostname = "ezconf.tp1.efrei"

  # Configuration des ressources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.customize ["createhd", "--filename", "ezconf.tp1.efrei_#{Time.now.to_i}.vdi", "--size", 20 * 1024]#20GB
  end

  # Provisioning avec un script bash
  config.vm.provision "shell", path: "script.sh"

end
```
**Repackaging**

```
  # Repackaging de la VM
  config.vm.provision "shell", inline: "vagrant package --output rocky-efrei.box"
```

**Vagrantfile plusieurs VMs**
```
Vagrant.configure("2") do |config|
  # VM node1.tp1.efrei
  config.vm.define "node1" do |node1|
    node1.vm.box = "generic/rocky9"
    node1.vm.hostname = "node1.tp1.efrei"
    node1.vm.network "private_network", ip: "10.1.1.101"
    node1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
  end

  # VM node2.tp1.efrei
  config.vm.define "node2" do |node2|
    node2.vm.box = "generic/rocky9"
    node2.vm.hostname = "node2.tp1.efrei"
    node2.vm.network "private_network", ip: "10.1.1.102"
    node2.vm.provider "virtualbox" do |vb|
      vb.memory = "1024" # 1G de RAM
    end
  end

  # Repackaging de la VM
  config.vm.provision "shell", inline: "vagrant package --output rocky-efrei.box"
end
```

**Ping entre les VMs**
```
[vagrant@node1 ~]$ ping 10.1.1.102
PING 10.1.1.102 (10.1.1.102) 56(84) bytes of data.
64 bytes from 10.1.1.102: icmp_seq=1 ttl=64 time=41.3 ms
64 bytes from 10.1.1.102: icmp_seq=2 ttl=64 time=0.776 ms
64 bytes from 10.1.1.102: icmp_seq=3 ttl=64 time=0.979 ms
64 bytes from 10.1.1.102: icmp_seq=4 ttl=64 time=0.793 ms
^C
--- 10.1.1.102 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2997ms
rtt min/avg/max/mdev = 0.776/10.964/41.311/17.520 ms
```













