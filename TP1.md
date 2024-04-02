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







