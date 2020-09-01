# Creating multiple VMs using a single Vagrantfile


```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "debian/buster64"


  config.vm.define "nginx" do |nginx|
    nginx.vm.provider "virtualbox" do |vb|
       vb.memory = "512"
    end

    nginx.vm.network "private_network", ip: "192.168.33.10"
    nginx.vm.hostname = 'nginx'

    nginx.vm.provision "shell", inline: <<-SHELL
       apt-get update
       apt-get install -y nginx
    SHELL
  end

  config.vm.define "apache" do |apache|
    apache.vm.provider "virtualbox" do |vb|
       vb.memory = "512"
    end

    apache.vm.network "private_network", ip: "192.168.33.11"
    apache.vm.hostname = 'apache'

    apache.vm.provision "shell", inline: <<-SHELL
       apt-get update
       apt-get install -y apache2
    SHELL
  end
end
```
