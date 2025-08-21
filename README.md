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

* Download and install Git Bash.

Run: `ssh -i  <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>`

![Git bash instance connect](https://github.com/user-attachments/assets/b736b758-5a71-4375-800b-79cc22c16aef)

---
## 🔧 **STEP 1 ~ INSTALLING THE NGINX WEB SERVER**
* In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We'll use the apt package manager to install this package. Run:
* `sudo apt update`
* `sudo apt install nginx`

📌 To verify that nginx was successfully installed and is running as a service in Ubuntu.

Run: `sudo systemctl status nginx`

* If it is green and running, then you did everything correctly - you have just launched your first Web Server in the Clouds!

![sudo systemctl status nginx](https://github.com/user-attachments/assets/d8ed86cb-a16e-4f76-b6c6-9c1c97da4be5)

* We have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to op inbound connection through port 80:

📌 To check how we can access it locally in our Ubuntu shell.

Run: `curl http://localhost:80` or `curl http://127.0.0.1:80`

📌 To test how our Nginx server can respond to requests from the internet.

Open a web browser to access: `http://<Public-IP-Address>:80`

![Launch local ip on browser](https://github.com/user-attachments/assets/e2a649bb-7c13-4463-b5bf-fdb27ec3a018)


