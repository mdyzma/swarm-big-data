.. _vagrant:


Using vagrant
-------------

Vagrant is a command line tool to quickly create and nmanage virtual machines. Basic template is called box and is usually provided by third party i.e. Hashicorp or software producer.


Vagrantfile
-----------


This is Vagrantfile for Virtual Machine running on WIndows behind corporate proxy.


.. code-block:: ruby
    
    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    
    $theme = <<SCRIPT
    echo I am changing zsh theme...
    sed -i -e 's/^ZSH_THEME=.*/ZSH_THEME="agnoster"/' ~/.zshrc
    SCRIPT
    
    
    $docker = <<SCRIPT
    yum-config-manager --add-repo https://download.doc ker.com/linux/centos/docker-ce.repo 
    yum-config-manager --enable docker-ce-edge
    SCRIPT
    
    
    $post = <<SCRIPT
    systemctl start docker
    systemctl enable docker
    usermod -aG docker vagrant
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
      if Vagrant.has_plugin?("vagrant-proxyconf")
          config.proxy.http     = "http://warproxy.gmv.es:80"
          config.proxy.https    = "http://warproxy.gmv.es:80"
          config.proxy.no_proxy = "127.0.0.1"
        end
      
    
      ############################################################
      # Install section
      ############################################################
      # Install rpm prerequisites 
      config.vm.provision :shell, keep_color: true,
                                  name: "Updating...",
                                  inline: "yum -y -q update"
      # Install git
      config.vm.provision :shell, keep_color: true,
                                  name: "Installing git...",
                                  inline: "yum -y -q install git"
      # Install zsh
      config.vm.provision :shell, keep_color: true,
                                  name: "Installing zsh shell...",
                                  inline: "yum -y -q install zsh"
      # Install python-devel
      config.vm.provision :shell, keep_color: true,
                                  name: "Python development tools...",
                                  inline: "yum -y -q install python-devel"
      # Install pip repository for pip
      config.vm.provision :shell, keep_color: true,
                                  name: "Installing epel repository...",
                                  inline: "yum --enablerepo=extras install epel-release -y -q"
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
      config.vm.provision :shell, keep_color: true,
                                  name: "Installing docker...",
                                  inline: "yum -y -q install docker-ce"
    # Install docker post processing
      config.vm.provision :shell, keep_color: true,
                                  name: "Docker post processing...",
                                  inline: $post
    
      # config.vm.synced_folder '.', '/vagrant', disabled: true
      # # enable if you want to access the repo via a share
      # #config.vm.synced_folder './vagrant-git', '/tmp/vagrant-git'
    
    
      # # Set global http.proxy settings for git
      # config.vm.provision :shell, privileged: false,
      #   inline: "git config --global http.proxy http://warproxy.gmv.es:80"
    
      # # Set global https.proxy settings for git
      # config.vm.provision :shell, privileged: false,
      #   inline: "git config --global https.proxy http://warproxy.gmv.es:80"
    
      # Clone Oh My Zsh from the git repo
    
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
      ############################################################
    
      
      ############################################################
      # Developer Tools Install section
      ############################################################
      # config.vm.provision :shell, inline: "yum -y -q update"
      # config.vm.provision :shell, inline: "yum groups mark install 'Development Tools'"
      # # config.vm.provision :shell, inline: "yum groups mark convert 'Development Tools'"
      # config.vm.provision :shell, inline: "yum groupinstall 'Development Tools'"
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



