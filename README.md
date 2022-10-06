
___

PROJECT 2 : WEB-STACK-IMPLEMENTATION-LEMP-STACK-

---

This is a project which implement the use of Web Stack (LEMP STACK) In AWS. LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)

---

CREATING EC2 INSTANCE ON AWS

---

- Select region (the cl and launch a new EC2 instance of t2.micro family with Ubuntu Server

- Create a pair Key as the EC2 is created

- Move into the folder where the pair key is downloaded and run the following command to first change the mode

`sudo chmod 0400 <private-key-name>.pem`

- Use the command to connect to the instances

`ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>`

INSTALLING THE NGINX WEB SERVER

- we update the server package index with the command

`sudo apt update`

- we use the command to then get NGINX installed

`sudo apt install nginx`

- To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

`sudo systemctl status nginx`

- we try to access it locally on our ubuntu shell

        curl http://localhost:80

                or use 
                  
        curl http://127.0.0.1:80         

![nginx status](./Images/nginx_status_verification.png)

- You can retrieve your IP address uisng the command

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

- Go to a web browser and open the page using the IP address

`http://<Public-IP-Address>:80`

![nginx status](./Images/welcome_ngnix.png)

___

Installing MySQL

- To install the software we use apt using this command

`sudo apt install mysql-server`

- we run the command below to remove every default insecure settings that comes with mysql

`sudo mysql_secure_installation`

- After the installation is finished checked if you can login to MYSQL

`sudo mysql -t`

![mysql](./Images/mysql_server.png)

- To stop the server you can use

`exit`
___

Installing PHP

---

- Now we can install PHP to process code and generate dynamic content for the web server using the command

`sudo apt install php-fmp php-mysql`
___

Configuring Nginx to Use PHP Processor

___

# When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server.

- Create the root web directory for your_domain as follows

`sudo mkdir /var/www/projectLEMP`

- Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user

`sudo chown -R $ubuntu:$ubuntu /var/www/projectLEMP`

- Next, open a new configuration file in Nginx’s

`sudo nano /etc/nginx/sites-available/projectLEMP`

- Copy and paste the command into the open window 

          #/etc/nginx/sites-available/projectLEMP

                server {
                    listen 80;
                    server_name projectLEMP www.projectLEMP;
                    root /var/www/projectLEMP;

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


- When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.              

- Activate your configuration by linking to the config file from Nginx’s sites-enabled directory using the command below

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

- This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing

`sudo nginx -t`

![activate_verify](./Images/activate_and_verify_nginx.png) 

- To disable default Nginx host that is currently configured to listen on port 80 we use the command

`sudo unlink /etc/nginx/sites-enabled/default`

- Reload Nginx to apply the changes

`sudo systemctl reload nginx`

- Create an index.html file in that location so that we can test that your new server block works as expected

              sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

- Now go to your browser and try to open your website URL using IP address

`http://<Public-IP-Address>:80`

(http://<Public-IP-Address>:80)

