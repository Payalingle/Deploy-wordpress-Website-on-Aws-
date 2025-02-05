Go to the Amazon RDS console. Click “Create database”.

![image](https://github.com/user-attachments/assets/1e6124d1-380c-471c-9ada-e334bdfcfabc)

Select “MySQL” as the engine type.

![image](https://github.com/user-attachments/assets/32ec26b8-bcf6-4823-b55d-fd0515a59936)

Choose the “Free tier” template for “DB instance class”.

![image](https://github.com/user-attachments/assets/af4f8a8f-7253-4133-a1b0-4a5647b3ecc4)
Enter a unique name for the “DB instance identifier”.

Set the “Master username” and “Master password” for the database.

![image](https://github.com/user-attachments/assets/df288368-2613-47f0-b130-c1496c50805d)

Set the “Virtual Private Cloud (VPC)” and “Subnet group” to create the instance in. Leave the other settings at their default values.

![image](https://github.com/user-attachments/assets/f43bfa6c-4e0c-4ca1-a194-14f4743b896f)

![image](https://github.com/user-attachments/assets/1c8c434c-2dfd-49cf-baac-9886f5a38cde)

Choose ‘Default VPC’

![image](https://github.com/user-attachments/assets/5ebdc38a-6ff4-4bf5-920c-57dec97040a8)

![image](https://github.com/user-attachments/assets/45f0c896-7eb2-4f85-a0c2-6b60d6131e85)

Click on “Create database”

![image](https://github.com/user-attachments/assets/504a48cb-3af7-4275-bcf2-89298ee5fa5a)

Database is created.

![image](https://github.com/user-attachments/assets/bd2c99ac-6e60-4977-b084-6725174a1766)

To configure this WordPress site, you will create the following resources in AWS:
An Amazon EC2 instance to install and host the WordPress application.
Go to the Amazon EC2 console., Click “Launch Instance”, Choose a Linux AMI.
Choose an instance type, such as t2. micro, Choose a VPC and subnet.
Configure security group rules to allow inbound traffic on the appropriate port for the type of database you are using (e.g. port 3306 for MySQL).

![image](https://github.com/user-attachments/assets/c13678a9-fcf6-4af6-ba96-63fd26e27e18)

An Amazon RDS for MySQL database to store your WordPress data.
Choose the MySQL database you created, go to the Connectivity & Security tab in the display, and choose the security group listed in VPC security groups. The console will take you to the security group configured for your database.

![image](https://github.com/user-attachments/assets/abadcb4c-3dad-425b-bf1b-1b71affce2fc)

![image](https://github.com/user-attachments/assets/fa615330-a4bb-4e2e-9ed7-83cc1f711d00)

Select the Inbound Rules tab, then choose the Edit inbound rules button to change the rules for your security group.

![image](https://github.com/user-attachments/assets/5303173b-5018-42ab-914f-9c546aebbc98)

![image](https://github.com/user-attachments/assets/5cf582cb-ae4f-413f-b3df-f2eaa551d781)

Change the Type property to MYSQL/Aurora, which will update the Protocol and Port range to the proper values.

Choose the security group that you used for your EC2 instance.

![image](https://github.com/user-attachments/assets/3fead967-15f7-4646-ba2a-b50708dc120a)

SSH into your EC2 instance

![image](https://github.com/user-attachments/assets/9333f475-be81-402d-9977-7c032a45a2dc)

run the following command in your terminal to install a MySQL client to interact with the database.
----->  sudo apt install mysql-client-core-8.8

![image](https://github.com/user-attachments/assets/34b60eae-6e5d-4b4d-bcb7-4550dfea91ea)

Run the following command in your terminal to connect to your MySQL database. Replace “<user>” and “<password>” with the master username and password you configured when creating your Amazon RDS database. -h is host which is RDS database endpoint.
-------> mysql -h <rds-database-endpoint> -P <port-no> -u <user> -p <password>

![image](https://github.com/user-attachments/assets/a9fc808d-498b-4770-95e7-d335330f897f)

Finally, create a database user for your WordPress application and give the user permission to access the wordpress database.

Run the following commands in your terminal:

CREATE DATABASE wordpress;
CREATE USER 'wordpress' IDENTIFIED BY 'wordpress-pass';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpress;
FLUSH PRIVILEGES;
Exit

you should use a better password than wordpress-pass to secure your database.

![image](https://github.com/user-attachments/assets/5c7ea756-7f25-4599-9279-47532e1fabd6)

To run WordPress, you need to run a web server on your EC2 instance.

To install Apache on your EC2 instance, run the following command in your terminal:
------>  sudo apt-get install apache2

![image](https://github.com/user-attachments/assets/0bfc9e48-9e02-4e6d-a0c8-a4832f3262f4)

To start the Apache web server, run the following command in your terminal:

systemctl restart apache2
You can see that your Apache web server is working by browsing public-ip of your ec2 instance.

![image](https://github.com/user-attachments/assets/7eab01f7-78fc-496a-bee0-91fbb77d96ff)

Set the server and post your new WordPress app.

First, download and uncompressed the software by running the following commands in your terminal:

wget https://wordpress.org/latest.tar.g
tar -xzf latest.tar.gzz

![image](https://github.com/user-attachments/assets/64bd21f3-d6df-4443-a9ce-72e68b259ec3)

you will see a tar file and a directory called WordPress with the uncompressed contents using the ls command.

![image](https://github.com/user-attachments/assets/9ad13a1a-ff55-45b3-a021-76e921e92a70)

Change the directory to the WordPress directory and create a copy of the default config file using the following commands

open the wp-config.php file

![image](https://github.com/user-attachments/assets/d2366d36-6603-4650-be8c-6c3f6bd8c77e)

Edit the database configuration by changing the following lines:

![image](https://github.com/user-attachments/assets/4accffe1-9208-47e6-b94b-d00766208e55)

DB_NAME: your RDS database name
DB_USER: The name of the user you created in the database in the previous steps
DB_PASSWORD: The password for the user you created in the previous steps
DB_HOST: The hostname of the database means your database endpoint
The second configuration section you need to configure is the Authentication Unique Keys and Salts.

You can replace the entire content in that section with the below content:

define('AUTH_KEY',         '@VZ<pXEL?vb-kiz(Zfp_R9f9|.+T-O/P$Z9|T-q%~|KX@,/(RZk00K{ybHA=6nT6');
define('SECURE_AUTH_KEY',  '12Ip[Ts<IA>Vc+R#_X>i85OjMMRtks-o^E2(,$P[Q=f~Zt:@FrW1r$,` vqs|%@|');
define('LOGGED_IN_KEY',    'o*_:obJ!+wtc8&]QhK}-xEVv+eVD!hFbBzkxKn@}(gK{-{d|l-?9b)8+)tfx8zjl');
define('NONCE_KEY',        'Ue<fX0Z71vg7Y&F+~CqM-G%N~ozMe%?qrp-@|tTVh??zJ4:~Sm,VhTKBE0C7DY(?');
define('AUTH_SALT',        'v.Z^1,QR66F-CDW=t<daxxk-;|M3cC{XzF`rn#l[U]f-fboHZYY/c8nvYU(uM`a]');
define('SECURE_AUTH_SALT', 'Cz$[Mq>)Hc=BSo,&Q%;,r}Eu7!:>nj4N91WeIx|7jp=fc+S64lMXCNj|h&a9Q5[D');
define('LOGGED_IN_SALT',   'Y{[of`B<!<<Za+|YtiMJkd33@a}+I%-0u}EPmAx?hW~$_(( u iuIJ2UQUJIv7(Q');
define('NONCE_SALT',       'g<!-Nhqy2E{X{87!|x{Amg3v:Z%e8d(z3l9x|g-T uB62u4xuw?uyjS_>1Yl]~Kw');

![image](https://github.com/user-attachments/assets/7ff506cd-2e6f-40a9-b4c3-32ef6afed1de)

First, install the application dependencies you need for WordPress. In your terminal, run the following command.

sudo apt install php libapache2-mod-php php-mysql -y

![image](https://github.com/user-attachments/assets/e23e18fb-3f59-4992-ad15-c67b1f4fd039)

Copy your WordPress application files into the /var/www/html directory used by Apache.

sudo cp -r wordpress/* /var/www/html/

![image](https://github.com/user-attachments/assets/41e96cf9-beab-4e3a-8b06-0d96e5e88590)

Finally, restart the Apache web server

systemctl restart apache2
Browse “ec2-public-ip/wp-admin/” and you should see the WordPress welcome page.

![image](https://github.com/user-attachments/assets/f65ba8ed-bf36-45c9-a09b-027cd3094758)










