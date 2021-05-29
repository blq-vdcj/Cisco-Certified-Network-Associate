# nagios installation raspberry pi

## ***In this section, we will be preparing your Raspberry Pi to compile and run the Nagios software.***

### **1. Before we start, let us ensure that our operating system is entirely up to date.**

To update everything, we need to type in the following two commands into the terminal.

    sudo apt update
    sudo apt full-upgrade

### **2. Once your Raspberry Pi has finished updating, we can now install the packages that we will be using to run Nagios.**

Run the following command to install all the packages we require.

    sudo apt install -y autoconf build-essential wget unzip apache2 apache2-utils php libgd-dev snmp libnet-snmp-perl gettext libssl-dev wget bc gawk dc libmcrypt-dev

This command installs several packages that we need. These packages include the compiler we need to compile the Nagios software. We also install the Apache web server, which is used for Nagios’s web interface.









## ***Downloading and Compiling Nagios***
### **In this section, we will walk you through the quick few steps needed to compile the Nagios software on your Raspberry Pi.**


### **1. To start, we are going to first change into the /tmp directory.**

This directory is where we will download, extract, and compile the Nagios source code.

    cd /tmp


### **2. We can now download the Nagios source code to our Raspberry Pi by running the following command.**

    wget -O nagios.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.6.tar.gz

This command will use wget to download the Nagios source code to our /tmp directory.


### **3. Once the archive has finished download, we can extract it by running the following command.**

    tar xzf nagios.tar.gz


### **4. Now change into the Nagios directory and configure the software for compilation.**

    cd /tmp/nagioscore-nagios-4.4.6/
    ./configure --with-httpd-conf=/etc/apache2/sites-enabled


We pass in the path to where we want our apache2 configuration to be stored. This path will be utilized later in the tutorial.


### **5. Let us now compile Nagios by running the following command.**

    make all

This process can take some time as it needs to compile all the Nagios code. Running this on a Raspberry Pi 4 took approximately 5 minutes.









## ***Setting Up Nagios on the Raspberry Pi***
### **In this section, we will be showing you how to set up Nagios on your Raspberry Pi. The next few steps will finish up the setup process for Nagios.**


### **1. Let us make use of the make command to create the user and group Nagios needs to run.**

We will also add the www-data user to the nagios group that is created by our make command.

    sudo make install-groups-users
    sudo usermod -a -G nagios www-data

### **2. Next, install the compiled binaries to our operating system by utilizing the command below.**

    sudo make install

### **3. We can also use the make command to install the Nagios service and set it up to start at boot.**

Run the following command to install the Nagios core daemon.

    sudo make install-daemoninit

### **4. Now we can run the following command to set up the external command directory.**

    sudo make install-commandmode

### **5. Our next step is to copy the sample configuration file again by using the make command.**

    sudo make install-config

These configuration files are needed for Nagios to operate. Without the config files, the software will not load.

### **6. Our second last step is to install the Apache configuration files.**

This command will install the required configuration files to the directory we specified when we configured the makefile.

We will also use two a2enmod commands to make sure that the required Apache modules are enabled.

    sudo make install-webconf
    sudo a2enmod rewrite
    sudo a2enmod cgi

### **7. In our final step, we will be creating an Apache user that you will use to access the Nagios interface on your Raspberry Pi.**

The following command will create a user called nagiosadmin. You will be asked to specify a password for this user.

    sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin

The user must be called nagiosadmin to satisfy the default configuration of Nagios.










## ***Starting Nagios on the Raspberry Pi***
### **In this section, we will be walking you through how to start the Nagios software on your Raspberry Pi.**



### **1. Our first step is to restart the Apache web server by running the following command.**

    sudo systemctl restart apache2

Restarting Apache will allow our new configuration files to be loaded in.



### **2. Next, enable the Nagios service and start it up by running the following two commands.**

    sudo systemctl enable nagios
    sudo systemctl start nagios

By enabling the service, we will be allowing Nagios to start up at boot on your Raspberry Pi.



### **3. You can verify that Nagios has started on your Raspberry Pi by running the following command.**

    sudo systemctl status nagios

If everything is working as intended, you should see the following text be outputted to the terminal.

    Active: active (running)

This text tells us that the service is active and is currently running.









## ***Installing the Nagios Plugins***

For Nagios to work correctly, we need  to install its plugins. Without it, the Nagios software will have very little functionality.
### **In this section, we will be showing you how to install these Nagios plugins on your Raspberry Pi.**

### **1. First change into our /tmp directory.***

    cd /tmp


### **2. Now that we are in the /tmp directory, we can download the Nagios plugins by running the command below.**

    wget -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/releases/download/release-2.3.3/nagios-plugins-2.3.3.tar.gz


### **3. Now extract the plugin source code to our current directory by using the following command.**

    tar zxf nagios-plugins.tar.gz


### **4. Our next step is to change into our newly created directory and configure the plugins for compilation.**

    cd /tmp/nagios-plugins-2.3.3
    ./configure


### **5. Once the configuration process has completed, we can compile the Nagios plugins by running the following command.**

    make

Depending on your Raspberry Pi, this process can take some time. However, it should be a lot faster than compiling the base Nagios code.


### **6. Finish up this process by installing the Nagios plugins by running the following command.**

    sudo make install


### **7. To make sure Nagios loads in the new plugins, restart the software by running the command below.**

    sudo systemctl restart nagios










## ***Connecting to the Nagios Web Interface***

Now that we have installed everything, we can now test that the Nagios web interface is online and functioning.




### **1. To access the Nagios web interface, you will need to know the Raspberry Pi’s IP address.**

You can retrieve your Raspberry Pi’s local IP address by running the following command.

    hostname -I

### **2. To connect to the Nagios web interface, you will need to go to your Pi’s IP address followed by /nagios.**

    http://[YOURIPADDRESS]/nagios

When you try to connect, you will be asked to enter a username and password.

If you have been following this tutorial, the username should be nagiosadmin, and the password should be what you specified.

### **3. Upon a successful connection, you should be greeted by the Nagios core homepage, all running from your Raspberry Pi.**


