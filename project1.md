## Web stack implementation (LAMP)
I logged in via ssh using gitbash. I updated the server and installed apache
- sudo apt update
- sudo apt install apache2
 
I confirmed the status using 'sudo systemctl status apache2' It was showing green indication success.

<img width="576" alt="APACHE STTUS CHECKED" src="https://github.com/Itsjustfemi/darey.ioprojects/assets/98546783/2eee8008-6247-46e6-89b9-e98ba2e09230">
TCP port 80 (https) is already opened up in my inbound rule with IP 0.0.0.0/0 (which means all IP)
I opened my google browser and pasted the below. I saw the apache homepage.
http://<Public-IP-Address>:80
  
![apache2 default page](https://github.com/Itsjustfemi/darey.ioprojects/assets/98546783/9ed5eb4b-aeb1-44de-9b48-39c6d4e2c44f)
- **INSTALLING MYSQL**
  I ran the below cmd to install the mySQL database
sudo apt install mysql-server
Below is to run a security script that comes pre-installed with MySQL (press Y for everything)
sudo mysql_secure_installation

I ran the below cmd to install the mySQL database
sudo apt install mysql-server
Below is to run a security script that comes pre-installed with MySQL (press Y for everything)
- sudo mysql_secure_installation
  
  ![sudo mysql](https://github.com/Itsjustfemi/darey.ioprojects/assets/98546783/5761c56c-89db-4e84-af8d-8d8c357c4031)
  
 At some point i was asked to select a password MySQL root userand I did.
WHen finished i entered the mysql console using the command below and also got the screenshot below;
- sudo mysql
  
**INSTALLING PHP**
  
  The cmd below installes:
PHP which is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, I"ll install php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Lastly, I"ll need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
- sudo apt install php libapache2-mod-php php-mysql
  ![php_version](https://github.com/Itsjustfemi/darey.ioprojects/assets/98546783/52c55de1-d521-4656-9100-a0ff12a8caa6)

  *CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE**
I will be using a domain called 'projectdomain.' Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.

I will be adding my own directory next to the default one with the cmd below:
  
  - sudo mkdir /var/www/projectdomain
 
Next cmd is to assign ownership of the directory

sudo chown -R $USER:$USER /var/www/projectdomain


create and open a new configuration file in Apache’s sites-available directory using vi

 sudo vi /etc/apache2/sites-available/projectdomain.conf
 
 This will create a blank new file. We are inputing the below after pressing "i"
 <VirtualHost *:80>
    ServerName projectdomain
    ServerAlias www.projectdomain 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectdomain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
  
  *Afterwwards, we Hit the esc button on the keyboard
Type :

Type wq. and Hit ENTER to save the file*

If we now use the ls cmd to view the file in the directory : 

- sudo ls /etc/apache2/sites-available
  
![checking projectlamf conf](https://github.com/Itsjustfemi/darey.ioprojects/assets/98546783/98a7f948-5933-4ce4-90e9-59b3af1e4283)
I used a2ensite command to enable the new virtual host:

sudo a2ensite projectlamp

I disabled the default site that comes with APache using:

sudo a2dissite 000-default

To make sure your configuration file doesn’t contain syntax errors,  I ran:
- sudo apache2ctl configtest and 
- sudo systemctl reload apache2 to reload it
  
  I Created an index.html file in that location so that we can test that the virtual host works as expected:
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html

WHen i tried going to the browser to access it. I got a message that contains exactly what i put in the cmd above (screenshot unavailable)

To ### ENABLE PHP ON THE WEBSITE# 
  we edit the /etc/apache2/mods-enabled/dir.conf file and change the order in
which the index.php file is listed within the DirectoryIndex directive:
  
  - sudo vim /etc/apache2/mods-enabled/dir.conf
  
  >IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
>/IfModule>

- After saving and closing the file, i reloaded Apache so the changes take effect:
sudo systemctl reload apache2

- Finally, I created a PHP script to test that PHP is correctly installed and configured on my server.
  
  vim /var/www/projectdomain/index.php
- it will open a blanc file and i put the belw cmd inside

- I refreshed my page and saw the following:
LAstly i removed the file because it contians sensitive information:
sudo rm /var/www/projectlamp/index.php
  
  ![final final php info page](https://github.com/Itsjustfemi/Projects/assets/98546783/1b999aae-2026-4da4-a2a7-be47d405b71c)
