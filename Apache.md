## Project 1 
### LAMB STACK IMPLEMENTATION Using AWS INFRASTRUCTURE
#### STEP 1 - Install Apache using Ubuntu's package manager
> update a list of packages in package manager

    sudo apt update

![updating package list](./images/capture.jpeg)
![updating package list](./images/Capture2.jpeg)

> run apache2 package installation

    sudo apt install apache2

![installing apache2](./images/Capture.PNG)

> verify apache is running as a service in our os

    sudo systemctl status apache2
![verifying apache2](./images/Image.PNG)

> updating the firewall on EC2 instance
![updating firewall](./images/AWS.PNG)

> Accessing the apache server locally

    curl http://localhost:80 or curl http://127.0.0.1:80
![verify_apache_locally](./images/Curl.PNG)

> Accessing the apache server publically

    http://<Public - IP -Address>:80 -syntax
    http://3.89.102.38:80 run on web browser
 ![Web server on the web](./images/web_server.PNG)

    curl -s http://3.89.102.38 run from terminal
![verify public address](./images/verify.PNG)

#### STEP 2 INSTALLING MYSQL
> Install DBMS(MYSQL)

      sudo apt install mysql-server
![installinig mysql](./images/install.PNG)
> log into MySQL

      sudo mysql
![logging into MySQL](./images/MYSQL.PNG)

> Set password for root user for security purposes

    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
![Password authentication](./images/Password.PNG)
> Exit MySQL shell

        mysql> exit
![exit mysql](./images/Exit.PNG)

> run script to remove insecure default settings and lock down access

         sudo mysql_secure_installation
![secure installation](./images/validate.PNG)
![validate image](./images/password%20validate.PNG)
![anon users](./images/anony.PNG)
![remote login](./images/Remote_login.PNG)
![reload](./images/Reload.PNG)

> Confirm login with password

        sudo mysql -p
![login beginning](./images/New_Password.PNG)
> log out of mysql

     mysql> exit
![logout](./images/New_e%3Dxit.PNG)


#### STEP 3 INSTALLING PHP
> Packages to enable PHP communicate successfully with Apache and MySQL

         sudo apt install php libapache2-mod-php php -mysql
![Installing PHP packages](./images/New_PackagePNG.PNG)
> Php Version

    php -v 
![php version](./images/Version.PNG)

At this point, your LAMP stack is completely installed and fully operational
- [x] Linux (Ubuntu)
- [x] Apache HTTP Server
- [x] MySQL
- [x] PHP

#### STEP 4 CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
>  Create a directory for a domain named projectlamp

       sudo mkdir /var/www/projectlamp
> Assign ownership with your current system user

       sudo chown -R $USER:$USER /var/www/projectlamp
> Create and open a new configuration file in Apache's sites-available directory 

         sudo vi /etc/apache2/sites-available/projectlamp.conf
         <<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    Save and close file
> Show the new file in the sites-available directory

         sudo ls /etc/apache2/sites-available
![showing the file](./images/LS.PNG)
> Enable new virtual host

          sudo a2ensite projectlamp
![virtual host](./images/Lamp.PNG)
> Disable Apache's default website 

        sudo aa2dissite 000-default
![disable default webpage](./images/disable_website.PNG)
> Confirm configuration is free from syntax error

         sudo apache2ctl configtest
![Check_Syntax_Error](./images/Syntax%20ok.PNG)
> Reload Apache for changes to take effect

         sudo systemctl reload apache2
New website is now active, but the web root/var/www/projectlamp is still empty
> Create an index.html file to test the virtual host works

         sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
> Open website URL using IP address

         http://52.87.180.61:80
![website](./images/Website.PNG)
> Open website with DNS name

         http://ec2-52-87-180-61.compute-1.amazonaws.com:80
![website using DNS](./images/DNS.PNG)


##### STEP 5 ENABLE PHP ON THE WEBSITE
> Open configuration file

         sudo vim /etc/apache2/mods-enabled/dir.conf
> Make changes on the configuration file

         <IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
      </IfModule>
Save and close the file
> Reload Apache web server

       sudo systemctl reload apache2
> Create a new PHP script to test that PHP is correctly installed and configured

        vim /var/www/projectlamp/index.php
> Paste this in the blank file

        <?php
        phpinfo();
Save and close the file
> Reload Webpage

![Php_info](./images/New_PHP.PNG)
![php_info](./images/New_PHP.2.PNG)

> Remove PHP sensitive information

      sudo rm /var/www/projectlamp/index.php








