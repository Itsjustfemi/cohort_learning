# LEMP STACK IMPLEMENTATION
I Started my Ec2 Instance and Connected through SSH `ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>`
. I updated the ubuntu server and also installed the Nginx.

`sudo apt update`

`sudo apt install nginx`


`sudo systemctl status nginx`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/ff18764f-fe06-44af-a6c0-00097c1db533)

I opened my browser and typed the public Ip of my EC2 http://:80 , I was able to see my nginx webserver installed

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/6509f5c0-b56d-49e2-9c4e-7859db6dfd36)

## Installing mysqlserver ##
 I used the apt cmd to install my web server
 `sudo apt install mysql-server`. WHen prompted, i selected 'y'
 
 ![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/ccb3c206-8dc0-4f61-9f31-dbe6e9a331ee)

I proceeded running some scripts to remove some insecure default settings and lock down access to your database system as below: 

`sudo mysql_secure_installation -y`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/ccd02389-3a71-4bc3-a9ca-984bbe4dd5a5)

Y was selected for every prompt. A password was prompted and selected for the MySQL root user. WHen through, I was able to log in to the MySQL console by typing:

`sudo mysql`

Below is the result. 
I proceeded to log out using the 'exit' cmd.

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/7502f549-d428-4dd0-97af-413f756cf0a9)

 ## Instaling PHP ##
 
 I installed PHP to process code and generate dynamic content for the web server. ALl at once, I installed php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
 
 'sudo apt install php-fpm php-mysql'
 
 When prompted, I typed Y and press ENTER to confirm the installation. 
 
 ### Configuring Nginx to Use PHP Processor ###
 When using the Nginx web server, we can create server blocks to encapsulate configuration details and host more than one domain on a single server. I used *Projectdomain* as my sample domain.
 
I created a directory structure within /var/www for my your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites. 
 I created the root web directory for my domain using the command below:

sudo mkdir /var/www/projectdomain.
 The below command did the following with screenshot as reference below;
 
 1. Create the root web directory for my domain ================================ "sudo mkdir /var/www/projectdomain"
 
 2.Assign ownership of the directory with the $USER environment variable, which will reference my current system user: === "sudo chown -R $USER:$USER /var/www/projectdomain"
 
 3.open a new configuration file in Nginx’s sites-available directory using Nano == " sudo nano /etc/nginx/sites-available/projectdomain"
 
 ![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/1d000ffd-f346-47e0-b612-359af2649fda)
 
 The command below was pasted in the config. file
 
 #/etc/nginx/sites-available/projectdomain

server {
    listen 80;
    server_name projectdomain www.projectdomain;
    root /var/www/projectdomain;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
 
 As per the block code above, below is what they do.
 *listen* — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.
*root* — Defines the document root where the files served by this website are stored.
 
*index* — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.
 
*server_name* — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.
 
 When i was done typing the command, I saved and closed the file using nano CTRL+X and then y and ENTER to confirm. After then I Activated my configuration by linking to the config file from Nginx’s sites-enabled directory: using the command below:
 
 sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
 
 I ran the below code to test my cmd for syntax error and as shown, the result is sucessful.
 
 ![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/1de9e3cb-aef4-42b0-922f-c8e5c264722e)

The below 2cmds is to disable default Nginx host that is currently configured to listen on port 80; and reload Nginx to apply the changes:

`sudo unlink /etc/nginx/sites-enabled/default`


`sudo systemctl reload nginx`

sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectdomain/index.html

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/fa509fdd-1049-4801-9a0b-539da7e0707e)

### TESTING PHP WITH NGINX ###
I Opened a new file called info.php within my document root in my text editor to confirm Nginx can correctly hand .php files off to my PHP processor:

`sudo nano /var/www/projectdomain/info.php`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/9ce22f2b-4bd5-4416-9fa2-aa3888316040)


- Afterwards, I pasted the php code and saved it. (to return information about my server)
 
 `<?php
phpinfo();
`
- i tried the cmd on my browsser and got the welcome message

- I used below cmd to remove the file I created:

`sudo rm /var/www/projectdomain/info.php`

##RETRIEVING DATA FROM MYSQL DATABASE WITH PHP##

- The aim is to do a simple "To do list" and configure access to it, so the Nginx website 
would be able to query data from the DB and display it.

- I ran the command below to connect to mysql

`sudo mysql`

- To create a new database, I run the following command from your MySQL console:


 CREATE DATABASE `example_database`;        (the name of the database is example_database)


- The below command was to create a new user **example_user** 


`CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`
---

The below cmd was to give this user permission over the example_database database:
---

 `GRANT ALL ON example_database.* TO 'example_user'@'%';`


AFterwards, I exited.

mysql> exit

- I tested if the newuser has permission in the database using:

mysql -u example_user -p


mysql> SHOW DATABASES;

- I proceeded to create a todo list inserting the roles using *mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");*

 To confirm that the data was successfully saved to my table, I ran:


mysql>  

`SELECT * FROM example_database.todo_list;`

<br>

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/051937d2-c16f-44fa-bb82-82b3555c8633)

- I exited 

- Then i Created a new PHP file in my custom web root directory using vi

`nano /var/www/projectdomain/todo_list.php`

I pasted the code below


```<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $p*****d);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

Save and close the file when i was done editing.

I can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:


The image below shows PHP environment is ready to connect and interact with your MySQL server.

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/9c51ba2e-4cc4-407b-a670-ba99f48ff586)
