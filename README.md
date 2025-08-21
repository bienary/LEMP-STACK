# 🧱DevOps/Cloud Engineering > WEB STACK IMPLEMENTATION (LEMP STACK)
## 🛠️ **DEVELOPMENT**

The LEMP stack is a collection of open-source software components that work together to host and run dynamic websites and web applications. It is similar to the LAMP stack, but replaces Apache with Nginx, a high-performance web server.

> L → Linux: The operating system that provides the foundation.

> E → Nginx: The web server that efficiently handles client requests and delivers web content.

> M → MySQL: The relational database system for storing and managing application data.

> P → PHP: The server-side scripting language that powers the application logic.

---
## ⚙ **STEP 0 ~ PREPARING PREREQUISITES**
* To complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.
* Sign in to AWS free tier account.
![EC2](https://github.com/user-attachments/assets/3376bbcc-df53-46eb-ad93-c1bfff719636)
![Instance Success](https://github.com/user-attachments/assets/5f3a1f1a-84f1-41d5-8a13-7d04ef173bf3)
![Instance Page](https://github.com/user-attachments/assets/be6f03c3-7ff0-470f-9872-bdd1fbddb286)

* Download and install Git Bash. Run:

```bash
ssh -i  <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```

![Git bash instance connect](https://github.com/user-attachments/assets/b736b758-5a71-4375-800b-79cc22c16aef)

---
## 🖧 **STEP 1 ~ INSTALLING THE NGINX WEB SERVER**
* In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We'll use the apt package manager to install this package. Run:
```bash
sudo apt update
```
```bash
sudo apt install nginx
```

📌 To verify that nginx was successfully installed and is running as a service in Ubuntu.

```bash
sudo systemctl status nginx
```

* If it is green and running, then you did everything correctly - you have just launched your first Web Server in the Clouds!

![sudo systemctl status nginx](https://github.com/user-attachments/assets/d8ed86cb-a16e-4f76-b6c6-9c1c97da4be5)

* We have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to op inbound connection through port 80:

📌 To check how we can access it locally in our Ubuntu shell.

```bash
curl http://localhost:80` or `curl http://127.0.0.1:80
```

📌 To test how our Nginx server can respond to requests from the internet.

Open a web browser to access: `http://<Public-IP-Address>:80`

![Launch local ip on browser](https://github.com/user-attachments/assets/e2a649bb-7c13-4463-b5bf-fdb27ec3a018)

---

## 📁**STEP 2 ~ INSTALLING MYSQL**

* MySQL is a popular relational database management system. So we will use it in our project.
* You need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.

```bash
sudo apt install mysql-server
```

* confirm installation by typing Y, and then ENTER
* log in to the MySQL console

```bash
sudo mysql
```

![sudo mysql](https://github.com/user-attachments/assets/ef643888-5ae2-475d-b21a-68bcc1f4be0b)


* It's recommended that you run a security script that comes pre-installed with MySQL.

```bash
ALTERUSER'root'@localroot' IDENTIFIED WITH mysql_native_password BY'Password.1';
```


* Exit the MySQL shell with: `mysql> exit`

📌 Start the interactive script by running: 

```bash
sudo mysql_secure_installation
```

* This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.
* Answer Y for yes, or anything else to continue without enabling.

![sudo mysql secure installation](https://github.com/user-attachments/assets/df3c08cd-2d04-40b6-8da4-059566c6832c)

📌 Test if you're able to log in to the MySQL console by typing: 

```bash
sudo mysql -p
```

---

## 🐘 **STEP 3 ~ INSTALLING PHP**

* Install PHP to process code and generate dynamic content for the web server.
* You will need to install php-fpm and php-mysql.
* To install these 2 packages at once, Run:

```bash
sudo apt install php-fpm php-mysql
```

![sudo apt install php-fpm php-mysql](https://github.com/user-attachments/assets/98394b8e-b633-4401-8ee2-1739cc3c9df9)

---

## **STEP 4 ~ CONFIGURING NGINX TO USE PHP PROCESSOR**

* Create the root web directory for your_domain as follows:

```bash
sudo mkdir /var/www/projectLEMP
```

* Assign ownership of the directory with the $USER environment variable.

```bash
sudo chown -R $USER:$USER /var/www/projectLEMP
```

* Open a new configuration file in Nginx's sites-available directory using your preferred common-line editor.

```bash
sudo nano /etc/nginx/sites-available/projectLEMP
```

* 📌This will create a new blank file. Type in the following bare-bones configuration:

```bash
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
                fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}
```

* When you are done editing, save and close the file.
* Activate your configuration by linking to the config file from Nginx's sites-enabled directory:

```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```

* Test your configuration for syntax errors by typing:

```bash
sudo nginx -t
```

* We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

```bash
sudo unlink /etc/nginx/sites-enabled/default
```

* Reload Nginx to apply the changes:

```bash
sudo systemctl reload nginx
```

![sudo nginx -t](https://github.com/user-attachments/assets/03dfbbc7-f343-42eb-94bb-0a3c96788255)


* 📌The web root /var/www/projectLEMP is still empty. Create an index.html file in that location.

```bash
sudo bash -c "echo 'Hello LEMP from hostname $(curl -X PUT -H \"X-aws-ec2-metadata-token-ttl-seconds: 21600\" -s http://169.254.169.254/latest/api/token | xargs -I {} curl -H \"X-aws-ec2-metadata-token: {}\" -s http://169.254.169.254/latest/meta-data/public-hostname) with public IP $(curl -X PUT -H \"X-aws-ec2-metadata-token-ttl-seconds: 21600\" -s http://169.254.169.254/latest/api/token | xargs -I {} curl -H \"X-aws-ec2-metadata-token: {}\" -s http://169.254.169.254/latest/meta-data/public-ipv4)' > /var/www/projectLEMP/index.html"
```

* Open your website URL using IP address or Public DNS name.

```bash
http://<Public-IP-Address>:80 or http://<Public-DNS-NAME>:80
```

> Your LEMP Stack is now fully configured.

---


## 🕵🏻‍♂️ **STEP 5 ~ TESTING PHP WITH NGINX**

* Open a new file called info.php

```bash
nano /var/www/projectLEMP/info.php
```

* Type the following lines into the new file.

```bash
<?php
phpinfo();
```

* Access this page in your web browser.

```bash
http://Public_IP_Address/info.php
```

![PHP](https://github.com/user-attachments/assets/22f99343-6594-4268-8c0a-a9128413c772)

* Remove the file you created as it contains sensitive information about your PHP environment.
```bash
sudo rm /var/www/projectLEMP/info.php
```

---

## 🌍 **STEP 6 ~ RETRIEVING DATA FROM MYSQL DATABASE WITH PHP**

* Connect to the MySQL console using the root account:

```bash
sudo mysql
```

* Create a new database, Run:
```bash
mysql> CREATE DATABASE 'example_database';
```

* Create new user and grant full privileges on the database.

```bash
mysql> CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Password.1';
```

* Give this user permission.

```bash
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
```

* Exit MySQL shell : `mysql> exit`

* Test if the new user has the proper permission.

```bash
mysql -u example_user -p
```

* Confirm you have access.

```bash
mysql> SHOW DATABASES
```

* Creat a test table named todo_list.

```bash
CREATE TABLE example_database.todo_list (
	item_id INT AUTO_INCREMENT,
	content VARCHAR (255),
	PRIMARY KEY(item_id)
);
```

* Using different values.

```bash
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item is to learn");
	INSERT INTO example_database.todo_list (content) VALUES ("My second important item is to acquire new skills");
	INSERT INTO example_database.todo_list (content) VALUES ("My Third important item is to keep learning");
```

* Confirm the data was successful.

```bash
mysql> SELECT * FROM example_database.todo_list;
```

![mysql data](https://github.com/user-attachments/assets/75ab196e-c93f-4d40-9bc2-4fec0ffbffb4)

* Exit MySQl console : `mysql> exit`

* Create a PHP script that will connect to MySQL.

```bash
<?php
$user = "example_user";
$password = "PassWord.1";
$database = "example_database";
$table= "todo_list";

try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
    echo "<h2>TODO</h2><ol>";
    foreach ($db->query("SELECT content FROM $table") as $row) {
        echo "<li>" . $row['content'] . "</li>";
    }
    echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

![NANO TODO](https://github.com/user-attachments/assets/a47519ea-0360-4673-a772-47f0bdf77d51)

* Visit public IP address in your web browser.
```bash
http://<Public_domain_or_IP>/todo_list.php
```

![TODO LIST WEB](https://github.com/user-attachments/assets/52e7dd45-ba0f-49cd-8e23-b74e259ef318)


---

## **CONGRATULATIONS!**

Using Nginx as web server and MySQL as database management system, we have successfully built a flexible foundation for serving PHP websites and applications to our visitors.
