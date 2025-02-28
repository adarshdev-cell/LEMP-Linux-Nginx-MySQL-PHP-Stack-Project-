LEMP Stack Project - Azure VM Deployment

Project Overview

This project sets up a LEMP (Linux, Nginx, MySQL, PHP) stack on an Azure Virtual Machine (VM) to host a PHP-based application that interacts with a MySQL database. The project also includes security enhancements, SSL/TLS configuration, and performance optimizations.

Prerequisites

An active Azure account

A configured No-IP domain (devtaskteam.zapto.com)

SSH access to the Azure VM

Basic knowledge of Linux and MySQL

Setup Instructions

1. Create an Azure Virtual Machine

Log in to Azure Portal.

Navigate to Virtual Machines and create a new VM with the following specs:

OS: Ubuntu 22.04 LTS

Size: Standard B1s (or higher based on expected traffic)

Authentication: SSH Public Key or Password

Allow inbound ports for HTTP (80), HTTPS (443), and SSH (22).

2. Install LEMP Stack

Connect to the VM via SSH and run the following commands:

sudo apt update && sudo apt upgrade -y
sudo apt install -y nginx mysql-server php-fpm php-mysql

3. Configure MySQL Database
