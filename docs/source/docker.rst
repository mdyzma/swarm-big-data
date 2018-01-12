.. _docker:


Dockerfile
==========


This is dockerfile description



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
