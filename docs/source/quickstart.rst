.. _quickstart:

Quickstart
==========

To run system in less than **3 minutes**.

.. image:: _static/vagrant-centos7.gif



Vagrant HOST box
----------------


1. Install vagrant using [Windows installer](https://www.vagrantup.com/downloads.html)

2. Set system variable to allow installed vagrant reach plugins in internet.


.. code-block:: bash

    set http_proxy=http://warproxy.gmv.es:80


3. Install vagrant plugins:

.. code-block:: bash
    
    vagrant plugin install vagrant-reload
    
    vagrant plugin install vagrant-proxyconf


4. Get project files:

.. code-block:: bash
    
    git clone <swarmhd.git>


vagrant GUEST box
-----------------

Before start one can check vagrant work. Cd to the projects folder and see:

.. code-block:: bash
    
    vagrant status


    Current machine states:
    default                   poweroff (virtualbox)

    The VM is powered off. To restart the VM, simply run 'vagrant up'


1. To start VM run:

.. code-block:: bash

    vagrant up


First time vagrant will download all initial elements like basic box and all docker containers composing our system.

To login to the freshly created virtual machine type:


.. code-block:: bash
    
    vagrant ssh
