.. _wazuh_installation:

Wazuh HIDS
==========

Installation from sources
-------------------------

Wazuh team has developed an OSSEC fork, implementing new features to improve OSSEC manager capabilities. These modifications do not affect OSSEC agents. Meaning that, if you are looking to install an agent, you just need to run a standard OSSEC installation and do not need to follow next steps. Documentation to perform an standard OSSEC installation can be found :ref:`here <ossec_installation>`.

Now, if you are installing an OSSEC manager, we strongly recommend you to use our forked OSSEC version. It provides compliance support, extended logging, and additional management features. Some of these capabilities are required for the integration with :ref:`ELK Stack <ossec_elk>` and :ref:`Wazuh RESTful API <ossec_api>`.

To start with this installation, first we need to set up the compilation environment by installing development tools and compilers. In Linux this can easily be done using your distribution packages manager:

For RPM based distributions: ::

   $ sudo yum install make gcc git

If you want to use Auth, also install: ::

   $ sudo yum install openssl-devel

For Debian based distributions: ::

   $ sudo apt-get install gcc make git

If you want to use Auth, also install: ::

   $ sudo apt-get install libssl-dev

Now we are ready to clone our Github repository and compile the source code, to install OSSEC: ::

   $ cd ~
   $ mkdir ossec_tmp && cd ossec_tmp
   $ git clone https://github.com/wazuh/ossec-wazuh.git
   $ cd ossec-wazuh
   $ sudo ./install.sh

Choose ``server`` when being asked about the installation type and answer the rest of questions as desired. Once installed, you can start your OSSEC manager running: ::

  $ sudo /var/ossec/bin/ossec-control start

Here are some useful commands to check that everything is working as expected. You should get  a similar output in your system. ::

  $ ps aux | grep ossec
  root     31362  0.0  0.1  27992   824 ?        S    23:01   0:00 /var/ossec/bin/ossec-execd
  ossec    31366  0.1  0.4  29968  2960 ?        S    23:01   0:00 /var/ossec/bin/ossec-analysisd
  root     31370  0.0  0.1  19648   844 ?        S    23:01   0:00 /var/ossec/bin/ossec-logcollector
  root     31382  0.0  0.1  19800   808 ?        S    23:01   0:00 /var/ossec/bin/ossec-syscheckd
  ossec    31385  0.0  0.1  28140   832 ?        S    23:01   0:00 /var/ossec/bin/ossec-monitord
  root     31407  0.0  0.1   7832   876 pts/0    S+   23:02   0:00 grep ossec

  $ lsof /var/ossec/logs/alerts/alerts.json
  COMMAND     PID  USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
  ossec-ana 31366 ossec   10w   REG  202,0      245 274582 /var/ossec/logs/alerts/alerts.json

  $ cat /var/ossec/logs/alerts/alerts.json
  {"rule":{"level":3,"comment":"Ossec server started.","sidid":502,"groups":["ossec","pci_dss"],"PCI_DSS":["10.6.1"]},"full_log":"ossec: Ossec started.","hostname":"vpc-agent-debian","timestamp":"2015 Nov 08 23:01:28","location":"ossec-monitord"}

RPM packages
------------

Yum repository
^^^^^^^^^^^^^^

To add the Wazuh yum repository, depending on your Linux distribution, create a file named ``/etc/yum.repos.d/wazuh.repo`` with the following content:

For Amazon Linux AMI: ::

     [wazuh]
     name = WAZUH OSSEC Repository - www.wazuh.com
     baseurl = http://ossec.wazuh.com/el/7/x86_64
     gpgcheck = 1
     gpgkey = http://ossec.wazuh.com/key/RPM-GPG-KEY-OSSEC
     enabled = 1

For RHEL and CentOS (version EL5): ::

     [wazuh]
     name = WAZUH OSSEC Repository - www.wazuh.com
     baseurl = http://ossec.wazuh.com/el/$releasever/$basearch
     gpgcheck = 1
     gpgkey = http://ossec.wazuh.com/key/RPM-GPG-KEY-OSSEC-RHEL5
     enabled = 1

For RHEL and CentOS (versions EL6 or EL7): ::

     [wazuh]
     name = WAZUH OSSEC Repository - www.wazuh.com
     baseurl = http://ossec.wazuh.com/el/$releasever/$basearch
     gpgcheck = 1
     gpgkey = http://ossec.wazuh.com/key/RPM-GPG-KEY-OSSEC
     enabled = 1

For Fedora (versions 21, 22 or 23): ::

     [wazuh]
     name = WAZUH OSSEC Repository - www.wazuh.com
     baseurl = http://ossec.wazuh.com/fc/$releasever/$basearch
     gpgcheck = 1
     gpgkey = http://ossec.wazuh.com/key/RPM-GPG-KEY-OSSEC
     enabled = 1

Wazuh manager installation
^^^^^^^^^^^^^^^^^^^^^^^^^^

To install the OSSEC manager using Yum packages manager, run the following command: ::

     $ yum install wazuh-manager

On Fedora 23, to install the OSSEC manager with DNF packages manager, run the following command: ::

     $ dnf install wazuh-manager

Wazuh agent installation
^^^^^^^^^^^^^^^^^^^^^^^^

To install the OSSEC manager using Yum packages manager, run the following command: ::

     $ yum install wazuh-agent

On Fedora 23, to install the OSSEC manager with DNF packages manager, run the following command: ::

     $ dnf install wazuh-agent

.. note:: If it is your first installation from our repository, you will need to accept our repository GPG key when prompted during the installation. This key can be found at: `http://ossec.wazuh.com/key/RPM-GPG-KEY-OSSEC <http://ossec.wazuh.com/key/RPM-GPG-KEY-OSSEC>`_ also if you don't have the EPEL-RELEASE repository, these packages will install to resolve dependences.
