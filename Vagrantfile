# -*- mode: ruby -*-
# vi: set ft=ruby :

$groupinstall = <<SCRIPT
yum group mark install "Development Tools"
yum groupinstall -y -q "Development Tools" 
SCRIPT

$theme = <<SCRIPT
echo I am changing zsh theme...
sed -i -e 's/^ZSH_THEME=.*/ZSH_THEME="agnoster"/' ~/.zshrc
SCRIPT

$docker = <<SCRIPT
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo 
yum install -y -q docker-ce
SCRIPT

$fonts = <<SCRIPT
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
cd ..
rm -rf fonts
SCRIPT

$post = <<SCRIPT
systemctl start docker
systemctl enable docker
usermod -aG docker vagrant
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  

  ############################################################
  # Proxy section
  ############################################################
  # if Vagrant.has_plugin?("vagrant-proxyconf")
  #     config.proxy.http     = "http://<company>:80"
  #     config.proxy.https    = "http://<company>:80"
  #     config.proxy.no_proxy = "127.0.0.1"
  #   end
  

  ############################################################
  # Install section
  ############################################################
  # Install git
  config.vm.provision :shell, keep_color: true,
                              name: "Installing git...",
                              inline: "yum -y -q install git"
  # Install zsh
  config.vm.provision :shell, keep_color: true,
                              name: "Installing zsh shell...",
                              inline: "yum -y -q install zsh"
  # Development tools
  config.vm.provision :shell, keep_color: true,
                              name: "Installing Development Tools Group",
                              inline: $groupinstall
  # Install python-devel
  config.vm.provision :shell, keep_color: true,
                              name: "Python development tools...",
                              inline: "yum -y -q install python-devel"
  # Install pip repository for pip
  config.vm.provision :shell, keep_color: true,
                              name: "Installing epel repository...",
                              inline: "yum install -y -q epel-release"
  # Install pip
  config.vm.provision :shell, keep_color: true,
                              name: "Installing python package manager...",
                              inline: "yum -y -q install python-pip"
  # Install wheel
  config.vm.provision :shell, keep_color: true,
                              name: "Installing python packaging tool wheel...",
                              inline: "yum -y -q install python-wheel"
# Install docker dependencies
  config.vm.provision :shell, keep_color: true,
                              name: "Installing docker dependencies...",
                              inline: "yum -y -q install device-mapper-persiste nt-data lvm2"
# Install docker repository
  config.vm.provision :shell, keep_color: true,
                              name: "Installing docker repository...",
                              inline: $docker
# Install docker
  # config.vm.provision :shell, keep_color: true,
  #                             name: "Installing docker...",
  #                             inline: "yum -y -q install docker-ce"
# Install docker post processing
  # config.vm.provision :shell, keep_color: true,
  #                             name: "Docker post processing...",
  #                             inline: $post
# Update packages  
  # config.vm.provision :shell, keep_color: true,
  #                             name: "Updating...",
  #                             inline: "yum -y -q grade"

  ############################################################
  # Configuration section
  ############################################################
  config.vm.provision :shell, privileged: false,
    inline: "git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh"

  # Copy in the default .zshrc config file
  config.vm.provision :shell, privileged: false,
    inline: "cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc"

  # Change the vagrant user's shell to use zsh
  config.vm.provision :shell, inline: "chsh -s /bin/zsh vagrant"
  
  # Install fancy fonts for zsh
  config.vm.provision :shell,  privileged: false, inline: $fonts
  # Change theme
  config.vm.provision :shell,  privileged: false, inline: $theme
  # Docker post processing
  config.vm.provision :shell, inline: $post

end
