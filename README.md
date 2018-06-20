Linux-Server-Configuration
In this project I deploy my build an item catalog project to an instance using a Linux server instance called Amazon Lightsail #### Process: 1)Create an amazon account. 2)Create an instance in lightsail.aws.amazon.com 3)Download putty.exe,puttygen.exe in puti.org 4)create static ip in lightsail and download it
Project Overview
Your task is to create a Item-Catlog that show category Items and thier Information clearly.This Project need Python, Postgrsql, Flask frame work as Backend and HTML, CSS/Bootsrap as frontend.we have to create own linux server and deploy catlog project using lamp.

Table Of Contents
Demo
Software Requirements
Installation
Output
References
Bug And Feature Requests
Demo
To see run project instantly click here.
Software Requirements
AWS account with lightsail service activated.
Python, Pip, SQLAlchemy, Apache2.
Flask, Virtualenv, Requests, Oauth2client.
Installation
Step 1:
Create Instance:

To Create Instance follow steps.
Sign in to aws console and from All Services, select lightsail.
Now click on create Instance button.
select as in image.
now click on create button.you will see this page.
Step 2:
Server Config:

Go to top right menu Click on Accounts.you will see this page. Download default key on to desktop.

Go to Home, then Networking from menu.Now Create static ip.

Download putty for better performance.like no server disconnections etc...

Install putty and open puttyGen.click on load.

select file downloaded from shh-keys eg: LightsailDefaultPrivateKey-ap-south-1.pem

click ok.then save private, yes, give some name.. save on desktop.then close it.

now you will see a .ppk file on desktop.

Open Putty. give staticIP of instance as hostname.

From left tree, click on SSH. after, Auth. You will see browser button.

click on browser, and select .ppk file and click on open.

press No. username ubuntu

Now you see linux environment Demo.

Step 3:
Create User Grader & Config:

Use following commands,

update/upgrade all packages
   ubuntu@ip-172-26-0-113:~$ sudo apt update  
create user

ubuntu@ip-172-26-0-113:~$ sudo adduser grader

Grant Permissions
ubuntu@ip-172-26-0-113:~$ sudo nano /etc/sudoers

Below the Root user append the following line demo.
grader ALL=(ALL:ALL) ALL

Step 4:
Generate ssh key pair for grader:

open command prompt, use command ssh-keygen

press enter, again enter, again enter, again enter.This will generate public and private ssh keys

Now go to local disk C, and User, your computerName, open .ssh file.

You will see id_rsa and id_rsa.pub.

open id_rsa.pub and keep aside.

Step 5:
Connect to grader using privateKey

In Command prompt, use command to connect to linux machine via SSH to user grader with private key generated.(id_rsa) C:\Users\vijaybabu>ssh -i .ssh/id_rsa grader@13.126.254.104
next to grader@ use your staticIp of Instance.
Step 6:
Changing SSH port to 2200

use command to edit sshd config file. grader@ip-172-26-0-113:~$ sudo nano /etc/ssh/sshd_config
Now in this,change port from 22 to 2200.demo
restart server ssh using command, grader@ip-172-26-0-113:~$ sudo service ssh restart
Note: Before Logging using ssh add custom TCP port 2200 and add custom UDP port 123 under lightsail firewall in networking tab in lightsail instance console.demo
Now login, using private key[id_rsa] with port 2200 demo.
C:\Users\vijaybabu>ssh -i .ssh/id_rsa -p 2200 grader@13.126.254.104

Step 7:
Disabling ssh login as root

grader@ip-172-26-0-113:~$ sudo nano /etc/ssh/sshd_config

make change PermitRootLogin Prohibited Pasword to no
Step 8:
Configurating UFW Firewall

Below commands will allow all required Ports and Enables the ufw.
   sudo ufw allow 80/tcp
   sudo ufw allow 123/udp
   sudo ufw enable
check ufw status with command, grader@ip-172-26-0-113:~$ sudo ufw status
Step 9:
Change Time Zone to UTC

Use command, sudo dpkg-reconfigure tzdata
click enter, again enter. its show,
   Current default time zone: 'Etc/UTC'
   Local time is now:      Tue Jun 19 15:40:48 UTC 2018.
   Universal Time is now:  Tue Jun 19 15:40:48 UTC 2018.
Step 10:
Apache2 Server Setup:

use below commands to install apache2, grader@ip-172-26-0-113:~$ sudo apt-get install apache2
for mod_wsgi, sudo apt-get install python-setuptools libapache2-mod-wsgi
To Enable mod_wsgi, use command grader@ip-172-26-0-113:~$ sudo a2enmod wsgi
NOTE: Dont forget to restart server apache2 if any changes made to it.

Step 11:
Creating FlaskApp

use following commands,
  grader@ip-172-26-0-113:~$  cd /var/www 
  grader@ip-172-26-0-113:~$  sudo mkdir FlaskApp
Now, move to FlaskApp, grader@ip-172-26-0-113:/var/www$ cd FlaskApp

tree looks like,

 |----FlaskApp
 |---------html
Step 12:
Install Git

To install git use command, grader@ip-172-26-0-113:/var/www/FlaskApp$ sudo apt-get install git
Step 13:
Install PostgreSQL

Install postgresql and setup, use commands grader@ip-172-26-0-113:/var/www/FlaskApp$ sudo apt-get install postgresql
login to postgres user and psql shell,
grader@ip-172-26-0-113:/var/www/FlaskApp$ sudo -i -u postgres psql

Create new user catalog with password 'password', use commands
CREATE USER catalog WITH PASSWORD 'password';

Permissions for user to createdb,
ALTER USER catalog CREATEDB;

Create a database named catalog with OwnerUser catalog,
CREATE DATABASE catalog WITH OWNER catalog;

Switch to database catalog,
\c catalog

Revoke all permission to public ,
REVOKE ALL ON SCHEMA public FROM public;

Give schema permission to user catalog ,
GRANT ALL ON SCHEMA public TO catalog;

close and exit from postgres user,
    postgres@ip-172-26-0-113:~$ exit
    grader@ip-172-26-0-113:~$
Step 14:
Use Git Repository Catalog project file

In that direcory clone your github repository grader@ip-172-26-0-113:/var/www/FlaskApp$ sudo git clone your Github Repository Link
Rename your repository to FlaskApp, using command grader@ip-172-26-0-113:/var/www/FlaskApp$ sudo mv repositoryName FlaskApp
Now move to respository Directory, Rename your project file to init.py using command grader@ip-172-26-0-113:/var/www/FlaskApp/FlaskApp$ sudo mv projectfileName.py __init__.py
I Occured an Error, While accesssing the client_secrets.json file, To Solve that, i used to replace Client_ID with follow, grader@ip-172-26-0-113:/var/www/FlaskApp/FlaskApp/$ sudo nano __init__.py
   PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))
   json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
   CLIENT_ID = json.load(open(json_url))['web']['client_id']
Use json_url instead client_secrets.json in script.
Use this line to Replace database connection in init.py and dbSetup fileName.py engine = create_engine('postgresql://catalog:password@localhost/catalog')
NOTE catalog is user in postgres and 'password' is password of user catalog.
Now you are ready with your applicatiom.
Step 15:
Configure and Enable a New Virtual Host

use below command, edit conf file sudo nano /etc/apache2/sites-available/FlaskApp.conf
Now add below CODE in it.
<VirtualHost *:80>
	ServerName mywebsite.com
	ServerAdmin admin@mywebsite.com
	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
	<Directory /var/www/FlaskApp/FlaskApp/>
		Order allow,deny
		Allow from all
	</Directory>
	Alias /static /var/www/FlaskApp/FlaskApp/static
	<Directory /var/www/FlaskApp/FlaskApp/static/>
		Order allow,deny
		Allow from all
	</Directory>
	ErrorLog ${APACHE_LOG_DIR}/error.log
	LogLevel warn
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Enable the virtual host. sudo a2ensite FlaskApp
Disabling the default apache2 page. sudo a2dissite 000-default.conf
Step 16:
Create the .wsgi File

cd /var/www/FlaskApp
sudo nano flaskapp.wsgi 
Add the following code.
 #!/usr/bin/python
 import sys
 import logging
 logging.basicConfig(stream=sys.stderr)
 sys.path.insert(0,"/var/www/FlaskApp/")

 from FlaskApp import app as application
 application.secret_key = 'Add your secret key'
press ctrl+x to save, y and enter
Step 17:
Installing require modules

You can either install all modules on your machine or create a virtual environment for the project and install the modules grader@ip-172-26-0-113:/var/www/FlaskApp/FlaskApp$ sudo pip install flask sqlalchemy psycopg2 requests oauth2client
for details visit here.
Setting up your Google Oauth2 for details visit here.

Make these changes to your project in console.developers.google.com
replace IP by your StaticIP of Instance.
Javascript origin  http://IP.xip.io

redirect URI 

http://IP.xip.io/login

http://IP.xip.io/gconnect

http://IP.xip.io/callback
NOTE: xip.io is a free DNS which will be the same as using IP address Demo.
Step 18:
Restart server

sudo service apache2 restart

Output
-Use below server details to run project ItemCatalog via private server,

Server Static IP Address 13.126.254.104

Hosted site Url http://13.126.254.104.xip.io/
To sse Output Click me.
References
stack overflow to errors retriving.
Deploying flask application using postgres, i followed Digital Ocean.
