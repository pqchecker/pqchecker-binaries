# pqChecker/pqMessenger debian packages installation instructions
# (c) 2015-2017, Abdelhamid MEDDEB <abdelhamid@meddeb.net>

Those packages are only for GNU/Linux Redhat and compliants systems.
Tested on Centos 6 & 7.

----- (I) ------
pqChecker module
----------------

1) Install
----------
Require root privileges:

sudo rpm -iv <package-file-name> (ie pqchecker-2.0.0-1.el7.x86_64.rpm for CentOS 7)

2) Remove
----------
Require root privileges:

sudo rpm -ev pqchecker

3) Dependency
-------------
This package depend on <openldap-servers> package.

4) Binary files location
------------------------
/usr/lib64/openldap/pqchecker.so

------ (II) ------
pqMessenger module
------------------

1) Install
----------
Require root privileges:

sudo rpm -iv <package-file-name (ie  pqmessenger-2.0.0-1.el7.x86_64.rpm for CentOS 7)

After installation, create the keystore.jks file into /etc/openldap/pqchecker folder with:

keytool -genkey -alias pqmessenger -keyalg RSA -keystore keystore.jks -dname "CN=pqMessenger, OU=pqChecker, O=PPolicy, L=LDAPPPolicy, S=IDF, C=FR" -keysize 1024 -validity 365 -storepass <Password> -keypass <Password>

or using the pqmessenger-createkeystore.sh script provided with source code.

Then execute:

sudo chown ldap:ldap keystore.jks
sudo chmod 640 keystore.jks

This file must be copied to the configuration location of the JMS server.

2) Remove
----------
Require root privileges:

sudo rpm -r pqmessenger

3) pqMessenger starts parameters
--------------------------------
In file:
/etc/default/pqmessenger

especially
. JAVA_HOME: Java JRE location.
. LOG_HOME: log files location. require write privileges to slapd system user.
. CONFIG_HOME: settings files location. require read privileges to slapd system user.
. NATIVE_LIB_HOME: must be at /usr/lib64/openldap

4) JMS application server configuration
---------------------------------------
Default: 
IP address: 127.0.0.1 (local machine)
Port: 61616

May be modified in /etc/openldap/pqchecker/config.xml file

/etc/openldap/pqchecker/keystore.jks must be copied to JMS server configuration location

5) Start and stop pqMessenger
-----------------------------
At system boot/shutdown
or manually:

Redhat/CentOS 6: sudo service pqmessenger start|stop
Redhat/CentOS 7: sudo systemctl start|stop pqmessenger

6) Dependency
-------------
This package depend on: 
. <pqchecker> and <apache-commons-daemon-jsvc> packages.
. Java JRE

7) Binary files location
------------------------
/opt/pqmessenger/pqmessenger-2.0.0.jar
