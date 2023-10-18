# Module 6 - Setting up infrastructure for a 2-tier web application using Wordpress and the MySQL database in single instance
While configuring this setup, there are multiple steps involved.
# Table of content:
- [2-Tier Highly Available WordPress Deployment](#2-tier-highly-available-wordpress-deployment)
  - [1. Create a GCP Account](#1-create-an-aws-account)
  - [2. Deploy a networking stack](#2-deploy-a-networking-stack)
  - [3. Create an Instance](#3-create-an-instance)
  - [4. SSH into your Instance](#4-ssh-into-your-instance)
  - [5. Install the Apache Web Server to run PHP](#5-install-the-apache-web-server-to-run-php)
  - [6. Install PHP to run WordPress](#6-install-php-to-run-wordpress)
  - [7. Install MySQL for adding database](#7-install-mysql-for-adding-database)
  - [8. Install WordPress](#8-install-wordpress)
  - [9. Map IP Address and Domain Name](#9-map-ip-address-and-domain-name)
    - [Other Method: To change your WordPress site URL with the wp-cli](#other-method-to-change-your-wordpress-site-url-with-the-wp-cli)
   
## 1. Create an GCP Account
First, you need to create an account on GCP, where you have to set up your 2-tier web application..

## 2. Deploy a networking stack

Here, we need to configure our network as per our requirements. I am going to create a logically isolated network environment with the help of VPC.
#### VPC-There are multiple steps involved , while creating vpc-
- Give you vpc name
- Select IPv4 CIDR block
- Give your CIDR block size (CIDR block size must be between /16 and /28.)


After creating the VPC successfully, the next step is creating a subnet. Creating multiple subnets allows us to create logical network divisions between resources. Here, I am going to create two subnets named: - 
  - PrivateSubnet1CIDR with 10.10.1.0/24 CIDR
  
#### Select your VPC (we have created vpc in previous step )
- First subnet
  
  ![image](https://github.com/rutwiksquareops/RTD/assets/146915430/af7a1e2d-0e8c-4cd5-9beb-98b7ac30162d)


#### Create your NAT gateway

Create a   NAT gateway to make subnet pvt.
![image](https://github.com/rutwiksquareops/RTD/assets/146915430/0a78f46b-7100-4330-891f-d4fd2580bbf8)


## Now it's time to launch the E2 instance and attach your VPC to that.



Here i am using Ubuntu 22 image , instance type E2  .

Now edit your network configuration and attach your vpc and enable Auto-assign public IP.

Create your security group and add inbound security group rules (ssh for remote access and http for webapp), and launch your instance and do ssh.

### SSH for installation PHP , Wordpress and MySql database

#### Open a terminal and run this command:
      # sudo apt-get update
      
### Install Apache
Run this command to install the apache package on ubuntu:

      # sudo apt-get install apache2
### Start your Apache webserver

      # systemctl start apache2
### Verify Apache Installation 
   - open a web browser and type in the address bar http://server_ip_address
   - check the  status of your apache webserver

### Install MySql 
     # apt-get install mysql-server

### To start your MySQL, run this command:
    # systemctl start mysql

### Change the authentication method
    # ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YOUR_STRONG_PASSWORD';

### After executing the ALTER USER command, run the following command:
    # mysql> FLUSH PRIVILEGES;
### Installing PHP
    # sudo apt install php libapache2-mod-php php-mysql

###  Install WordPress On Ubuntu
    # sudo wget -c http://wordpress.org/latest.tar.gz
    # sudo tar -xzvf latest.tar.gz
### Then move the WordPress files
    # sudo mv wordpress/* /var/www/

### Now set appropriate permissions , It should be owned by the Apache2 user and group called www-data.
    # sudo chown -R www-data:www-data /var/www/wordpress
    # sudo chmod -R 775 /var/www/wordpress

###  Create WordPress Database On MySQL
    mysql> CREATE DATABASE dbname;
    mysql> CREATE USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'db_password';
    mysql> GRANT ALL ON dbname.* TO 'username'@'%';
    mysql> FLUSH PRIVILEGES;
    mysql> EXIT;

###  open the wp-config.php configuration file for editing
  
![data](https://github.com/amanravi-squareops/road-to-devops/assets/146931382/b9e27f72-237c-4f61-90d6-979dca21e0be)

    # sudo systemctl restart apache2
    # sudo systemctl restart mysql
    
### Open your web browser, then enter your
    http://ip_address

