# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  
  if Vagrant.has_plugin?("vagrant-proxyconf")
      config.proxy.http     = "http://warproxy.gmv.es:80"
      config.proxy.https    = "http://warproxy.gmv.es:80"
      config.proxy.no_proxy = "127.0.0.1"
    end
  
  config.vm.synced_folder "vagrant_resources/", "/home/vagrant/resources"
  # config.vm.provision "shell", path: "vagrant_ressources/preparations.sh"
  # config.vm.provision :reload
  # config.vm.provision "shell", path: "vagrant_ressources/bootstrap.sh"
  # config.vm.provision "shell", path: "vagrant_ressources/python.sh"
end
