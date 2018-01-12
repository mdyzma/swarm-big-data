# Windows 8.1 scenario

## Vagrant

Setting docker on Windows 8.1 is tricky. Therefore it is better to set new virtual machine with Linux and provision entire software in form of installed libraries and docker containers.

This process is even more painful if corporate proxy is set up. Proper docker setup is then very, very comples. It is better to use other solution. __Virtualization__.

SWARM mission virtual machine run by vagrant/virtualbox can help to automate this process to great extend. Our Windows machine will be called HOST, while running virtual machine will be GUEST.

Step by step procedure to run VM behind corporate proxy.

### Vagrant HOST box

1. Install vagrant using [Windows installer](https://www.vagrantup.com/downloads.html)

2. Set system variable to allow installed vagrant reach plugins in internet.


    ```set http_proxy=http://warproxy.gmv.es:80```

3. Install vagrant plugins:

    ```vagrant plugin install vagrant-reload```
    
    ```vagrant plugin install vagrant-proxyconf```

4. Get project files:

    ```git clone <swarm-big-data.git> ```

<!-- 4. set VAGRANT_HTTP_PROXY="http_proxy=http://warproxy.gmv.es:80"
5. set VAGRANT_NO_PROXY="127.0.0.1" -->

### vagrant GUEST box

Before start one can check vagrant work. Cd to the projects folder and see:

```vagrant status```


    Current machine states:
    default                   poweroff (virtualbox)

    The VM is powered off. To restart the VM, simply run 'vagrant up'

1. To start VM run:

    ```vagrant up```

First time vagrant will download all initial elements like basic box and all docker containers composing our system.

To login to the freshly created virtual machine type:


 3. vagrant ssh
 4. modify proxy
    a) sudo sed 's/^proxy//g ""sudo < /etc/yum.conf # zrobic sed do podmiany liniii zaczynajacej sie od proxy zeby wrzucic HTTP_PROXY
    b) dodac export HTTP_PROXY do .bashrc
    c) source ~/.bashrc
    d) echo “http_proxy=http://your_proxy:your_port” >> /etc/environment
    e) echo “https_proxy=http://your_proxy:your_port” >> /etc/environment
 5. sudo yum -y -q update

 6. sudo yum groups mark install "Development Tools"
 7. sudo yum groups mark convert "Development Tools"
 8. sudo yum groupinstall "Development Tools"

 6. sudo yum --enablerepo=extras install epel-release -y -q
 7. sudo yum install python-pip python-wheel python-devel -y -q
    a) sudo pip install -U -q pip 
 8. sudo pip install -q --proxy warproxy.gmv.es:80 -r /vagrant/requirements.txt
 9. sudo yum install -y -q yum-utils device-mapper-persiste nt-data lvm2
10. sudo yum-config-manager --add-repo https://download.doc ker.com/linux/centos/docker-ce.repo 
11. sudo yum-config-manager --enable docker-ce-edge
12. sudo yum-config-manager --enable docker-ce-tes
13. sudo yum -y -q install docker-ce
14. sudo systemctl start docker
15. sudo systemctl enable docker
16. sudo su
17. usermod -aG docker vagrant
18. exit
19. 

# Docker

