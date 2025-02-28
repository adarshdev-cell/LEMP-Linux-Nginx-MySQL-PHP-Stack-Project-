# LEMP Stack Project - Azure VM Deployment

## Project Overview
This project sets up a LEMP (Linux, Nginx, MySQL, PHP) stack on an Azure Virtual Machine (VM) to host a PHP-based application that interacts with a MySQL database. The project also includes security enhancements, SSL/TLS configuration, and performance optimizations.

## Prerequisites
- An active [Azure](https://portal.azure.com/) account
- A configured No-IP domain (`devtaskteam.zapto.com`)
- SSH access to the Azure VM
- Basic knowledge of Linux and MySQL

## Setup Instructions

### 1. Create an Azure Virtual Machine
1. Log in to [Azure Portal](https://portal.azure.com/).
2. Navigate to **Virtual Machines** and create a new VM with the following specs:
   - OS: Ubuntu 22.04 LTS
   - Size: Standard B1s (or higher based on expected traffic)
   - Authentication: SSH Public Key or Password
   
3. Allow inbound ports for HTTP (80), HTTPS (443), and SSH (22).

   ![Azure VM Setup](images/azurevm.png)



### 2. Login with SSH 

```bash
ssh  wpuser@52.172.186.224
```
 ![Azure VM Setup](images/loginwithssh.png)

### 3. Updating Packages on Azure Ubuntu VM using APT

```bash
 sudo apt update
```
 ![Azure VM Setup](images/updateapt.png)

### 4. Install Nginx on Ubuntu
```bash
sudo apt isntall nginx
```
When prompted, press Y and ENTER to confirm that you want to install Nginx. Once the installation is finished, the Nginx web server will be active and running on your Ubuntu server.
 ![Azure VM Setup](images/installnginx.png)

### 5. ufw file enabled
If you have the ufw firewall enabled, as recommended in our initial server setup guide, you will need to allow connections to Nginx. Nginx registers a few different UFW application profiles upon installation. To check which UFW profiles are available, run:
```bash
sudo ufw app list
```
![Azure VM Setup](images/ufwallow.png)

Write the address that you receive in your web browser and it will take you to Nginx’s default landing page:
```bash
http://server_domain_or_IP
```
![Azure VM Setup](images/localonweb.png)

If you receive this page, you have successfully installed Nginx and enabled HTTP traffic for your web server.

### 6. Install MySQL

Now that you have a web server up and running, you need to install the database system to store and manage data for your site. MySQL is a popular database management system used within PHP environments.

```bash
sudo apt install mysql-server
```
![Azure VM Setup](images/installmysql.png)

When the installation is finished, it’s recommended that you run a security script that comes pre-installed with MySQL.

```bash
sudo mysql_secure_installation
```
If you answer “yes”, you’ll be asked to select a level of password validation.
![Azure VM Setup](images/mysqlyes1.png)

When you’re finished, test if you’re able to log in to the MySQL console:
```bash
sudo mysql
```
![Azure VM Setup](images/undermysql.png)


### 7. Installing PHP
```bash
sudo apt install php8.1-fpm php-mysql
```
When prompted, press Y and ENTER to confirm the installation.
You now have your PHP components installed. Next, you’ll configure Nginx to use them.

### 8. Configuring Nginx to Use the PHP Processor
```bash
sudo mkdir /var/www/your_domain
```
Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

```bash
sudo chown -R $USER:$USER /var/www/your_domain
```
Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:

```bash
sudo nano /etc/nginx/sites-available/your_domain
```
```bash
server {
    listen 80;
    server_name devtaskteam.zapto.org;
    root /var/www/devtaskteam;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock; # Adjust version if needed
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```
