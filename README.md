# Apache2 Configuration  on Linux #

---------------------------------

## Some commands to operate apache2 ##

Restart

```bash
sudo systemctl reload apache2
```

Stop

```bash
sudo systemctl stop apache2
```

Start

```bash
sudo systemctl start apache2
```

Activate new conf file in sites-available

```bash
sudo a2ensite *.conf
```

Enable conf file

```bash
sudo service apache2 reload
```

### Download & Installation ###

#### install apache2 server on system ####

```bash
sudo apt update
sudo apt install apache2
```

#### creating your Own Website ####

- html

```bash
/var/www/html
```

- setting can be edited

```bash
/etc/apache2/sites-enabled/ 000-default.conf
```

> We can modify how Apache handles incoming requests and have multiple sites running on the same server by editing its Virtual Hosts file.

#### cgi enable & configuration ####

```bash
# enable cgi
sudo a2enmod cgi
sudo service apache2 restart
# next move
sudo mkdir /var/www/gci/
cd /var/www/gci/
nano index.html
```

#### Troubleshoot: cgi-bin configuration ####

Default site setting location

```bash
cd /etc/apache2/conf-available/
nano serve-cgi-bin.conf
# serve-cgi-bin.con -> default configuration let's configure custom
<IfModule mod_alias.c>
 <IfModule mod_cgi.c>
  Define ENABLE_USR_LIB_CGI_BIN
 </IfModule>

 <IfModule mod_cgid.c>
  Define ENABLE_USR_LIB_CGI_BIN
 </IfModule>

 <IfDefine ENABLE_USR_LIB_CGI_BIN>
  ScriptAlias /cgi-bin/ /var/www/cgi-bin/
  # /var/www/cgi-bin/ ---> provide your path 1. ScriptAlias 2. Directory
  <Directory "/var/www/cgi-bin/">
   AllowOverride None
   Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
   Require all granted
  </Directory>
 </IfDefine>
</IfModule>
```

Create symbolic link to enable it

```bash
cd /etc/apache2/sites-enabled
# if serve-cgi-bin.conf exits delete it othervise it will create error
sudo ln -s ../sites-available/serve-cgi-bin.conf
```

Enable CGI Module Mannualy. it won't enable by default

```bash
cd /etc/apache2/mods-enabled
sudo ln -s ../mods-available/cgi.load
```

Reload Apache Configuration

```bash
sudo service apache2 reload
```

#### Setting up the VirtualHost Configuration File ####

We start this step by going into the configuration files directory:

```bash
cd /etc/apache2/sites-available/
```

Since Apache came with a default VirtualHost file, let’s use that as a base. (gci.conf is used here to match our subdomain name):

```bash
sudo cp 000-default.conf gci.conf
```

Now edit the configuration file:

```bash
sudo nano gci.conf
```

We should have our email in ServerAdmin so users can reach you in case Apache experiences any error:

```bash
ServerAdmin yourname@example.com
```

We also want the DocumentRoot directive to point to the directory our site files are hosted on:

```bash
DocumentRoot /var/www/gci/
```

The default file doesn’t come with a ServerName directive so we’ll have to add and define it by adding this line below the last directive:

ServerName gci.example.com

This ensures people reach the right site instead of the default one when they type in gci.example.com.

Now that we’re done configuring our site, let’s save and activate it in the next step!

#### Activating VirtualHost file ####

After setting up our website, we need to activate the virtual hosts configuration file to enable it. We do that by running the following command in the configuration file directory:

```bash
sudo a2ensite gci.conf
```

You should see the following output

> Enabling site gci.
To activate the new configuration, you need to run:

```bash
service apache2 reload
root@ubuntu-server:/etc/apache2/sites-available#
```

To load the new site, we restart Apache by typing:

```bash
service apache2 reload
```

End result

Now is the moment of truth, let’s type our host name in a browser.

![alt text](https://ubuntucommunity.s3.dualstack.us-east-2.amazonaws.com/original/2X/7/7d6944922296826f70f27ec9b5eff67bd7f46158.png "Congrats it works!")
