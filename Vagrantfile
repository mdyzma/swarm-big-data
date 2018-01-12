# -*- mode: ruby -*-
# vi: set ft=ruby :

$theme = <<SCRIPT
echo I am changing zsh theme...
sed -i -e 's/^ZSH_THEME=.*/ZSH_THEME="agnoster"/' ~/.zshrc
SCRIPT

$fonts = <<SCRIPT
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
cd ..
rm -rf fonts
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  

  ############################################################
  # Proxy section
  ############################################################
  # if Vagrant.has_plugin?("vagrant-proxyconf")
  #     config.proxy.http     = "http://<proxy>:<port>"
  #     config.proxy.https    = "http://<proxy>:<port>"
  #     config.proxy.no_proxy = "127.0.0.1"
  #   end
  ############################################################

  ############################################################
  # Oh My ZSH Install section
  ############################################################
  # Install git and zsh prerequisites 
  config.vm.provision :shell, inline: "yum -y install git"
  config.vm.provision :shell, inline: "yum -y install zsh"

  # Clone Oh My Zsh from the git repo
  config.vm.provision :shell, privileged: false,
    inline: "git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh"

  # Copy in the default .zshrc config file
  config.vm.provision :shell, privileged: false,
    inline: "cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc"

  # Change the vagrant user's shell to use zsh
  config.vm.provision :shell, inline: "chsh -s /bin/zsh vagrant"
  
  # Install fancy fonts for zsh
  config.vm.provision :shell,  privileged: false, inline: $fonts
  # Change theme
  config.vm.provision :shell,  privileged: false, inline: $theme
  ############################################################

  
  ############################################################
  # Developer Tools Install section
  ############################################################
  config.vm.provision :shell, inline: "yum -y -q update"
  config.vm.provision :shell, 
    inline: "yum groups mark install 'Development Tools'"
  # config.vm.provision :shell, 
    # inline: "yum groups mark convert 'Development Tools'"
  config.vm.provision :shell, 
    inline: "yum groupinstall 'Development Tools'"
  ############################################################


  ############################################################
  # Python Tools Install section
  ############################################################
  # sudo yum --enablerepo=extras install epel-release -y -q
  config.vm.provision :shell, 
    inline "yum install python-pip python-wheel python-devel -y -q"
  config.vm.provision :shell, 
    inline, "pip install -U -q pip"
  ############################################################
  
  
  ############################################################
  # Docker HBase section
  ############################################################
  # sudo yum install -y -q yum-utils device-mapper-persiste nt-data lvm2
  # sudo yum-config-manager --add-repo https://download.doc ker.com/linux/centos/docker-ce.repo 
  # sudo yum-config-manager --enable docker-ce-edge
  # sudo yum-config-manager --enable docker-ce-test
  # sudo yum -y -q install docker-ce
  # sudo systemctl start docker
  # sudo systemctl enable docker
  # sudo su
  # usermod -aG docker vagrant
  ############################################################
end
