# Linux-Server-Configuration
In this project I deploy my build an item catalog project to an instance using a Linux server instance called Amazon Lightsail

### Process:

#### Step-1

1)Create an amazon account. 	

2)Create an instance in lightsail.aws.amazon.com	

3)Download putty.exe,puttygen.exe in puti.org 		

--Open Putty. give staticIP of instance as hostname.											
select file downloaded from shh-keys eg: LightsailDefaultPrivateKey-ap-south-1.pem							
click ok.then save private, yes, give some name.. save on desktop.then close it.						
now you will see a .ppk file on desktop.								
From left tree, click on SSH. after, Auth. You will see browser button.								
click on browser, and select .ppk file and click on open.									
press No. username ubuntu												
4)Create static ip in lightsail and download it.										
5)Open putty enter your ip and add private key address.										
6)Connect to grader by using the following command:									
	ssh -i path/to/privatekey -p 2200 grader@ip address										
7)Creating grader User:							
	ubuntu@ip-172-26-0-113:~$ sudo apt update 						
	sudo adduser grader											
Follow the following steps:												
	sudo nano /etc/sudoers									
Grant sudo permission to grader									
	grader  ALL=(ALL:ALL) ALL									
8)We have to create a ssh key pair for grader										
Open new terminal/command prompt and type 									
	ssh-keygen									
This will generate public and private ssh keys which is saved to .ssh folder	
9)Change ubuntu user to grader												
	sudo su - grader											
10)Create a new directory .ssh and new file authorized_keys in that directory							
	mkdir .ssh											
        sudo nano .ssh/authorized_keys										
11)Permissions:														
	chmod 700 .ssh										
	chmod 644 .ssh/authorized_keys											
700 will give read write and execute permission to user.						
644 prevent other user from writting in to file. Then restart ssh server						
12)sudo service ssh restart												
13)ssh -i .ssh/id_rsa grader@ipaddress 												
14)Now we have to change our port from 22 to 2200											
	sudo nano /etc/ssh/sshd_config											
Then change it.															
15)Important thing is disabling ssh login as root										
	sudo nano /etc/ssh/sshd_config													
	########## make change PermitRootLogin no											
16)Uncomplicated firewall(ufw) enbling:												
	sudo ufw allow 2200/tcp											
	sudo ufw allow 80/tcp							
	sudo ufw allow 123/udp								
	sudo ufw enable											
17)For viewing ports type:													
	sudo ufw status												
18)Changing time Zone:													
	sudo dpkg-reconfigure tzdata													
#### Step-2																				
##### Software Requirements:							
AWS account with lightsail service activated.											
Python, Pip, SQLAlchemy, Apache2.												
Flask, Virtualenv, Requests, Oauth2client.									
####### Installation Process for softwares:							
-- sudo apt-get install apache2												
-- sudo apt-get install python-setuptools libapache2-mod-wsgi									
-- sudo a2enmod wsgi										
-- cd /var/www													
-- sudo mkdir FlaskApp														
-- sudo apt-get install git													
-- sudo apt-get install python-pip virtualenv							
-- sudo virtualenv venv														
-- sudo pip install Flask										
-- sudo pip install postgresql oauth2client httplib2 requests psycopg2							
-- cd FlaskApp											
Rename your repository to FlaskApp to __init__.py								
-- sudo mv project.py __init__.py											
In that directory clone your github repository										
-- sudo git clone 'https://github.com/username/filename.git'									
Error : While accesssing the client_secrets.json file								
PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))										
json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')								
CLIENT_ID = json.load(open(json_url))['web']['client_id']							
Replace client_secrets.json with json_url in scriptfile							
-- sudo apt-get install postgresql								
-- sudo su - postgres															
-- psql														
Now we have to create user 													
-- CREATE USER catalog WITH PASSWORD 'password';											
create user													
-- ALTER USER catalog CREATEDB;														
ALTER USER															
-- CREATE DATABASE catalog WITH OWNER catalog;								
database created												
Switch to database catalog									
-- \c catalog                                                                                   	
-- REVOKE ALL ON SCHEMA public FROM public;							
REVOKE																	
-- GRANT ALL ON SCHEMA public TO catalog;												
\q and type exit													
Change the database connection in both database_setup.py and init.py as						
engine = create_engine('postgresql://catalog:password@localhost/catalog')						
#### Step-3														
Configure and Enable a New Virtual Host, then type the following code in the shell						
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
-- sudo a2ensite FlaskApp														
-- sudo a2dissite 000-default.conf										
###### .wsgi file									
-- sudo nano flaskapp.wsgi 									
Then add the below code												
#!/usr/bin/python						
 import sys										
 import logging														
 logging.basicConfig(stream=sys.stderr)												
 sys.path.insert(0,"/var/www/FlaskApp/")								
 from FlaskApp import app as application								
 application.secret_key = 'Add your secret key'											
 press ctrl+x and y							
 #### Step-4
 
 Go to google console and change homeurl to 												
 http://ipaddress.xip.io\callback												
 http://ipaddress.xip.io\gconnect													
 http://ipaddress.xip.io\login								
###### FINAL STEP:										

-- sudo service apache2 restart								
Then go to webbrowser and type your ipaddress finaly we get our project.							
****My server Details

    Server static IP Address 13.232.39.180								
****Grader Key 																
-----BEGIN RSA PRIVATE KEY-----												
MIIEogIBAAKCAQEAsjQ6MwF5H7/yGlQY+EVcK+atb3/oG7a+MavG2N2EvdOM3iSX								
6GjmckT1QljOxpRlLZGJ5H8aRgYitXR0xYdsHJa/AYfGp4i/k6+/U7bkvxSAXIE9						
DQrTrE3FyKFcrNkwFs3aeOYwslMYAbdw97cTLwsQdjl+J+J+zVUh/KQsBHtsNwrU							
7vplozXR0LNDXv6sddu7fGv5w1oJ6hEMTiI3qZd1KBZI9sjP4PZ7UkQ539etZ6Vg								
4cN3WssUlOrUNYM7MIgSXmFMVJiPL8KxY4SzS+ZF/R9ZFDezKbREPi71R1/jKivL					
YM5QhNatCCahKqN+4I02esTbO52jEvs1mfGsKwIDAQABAoIBAAe6UBPKKpB/6GXP							
481QZLDarga5yzz4bcMFqffZk1oQBHnVqGjBs8ycxO39n+nooYKaXxpzkJYcygCI								
bk/qkXuj5eCRHMJDIdursWZV9hF7OB3K1PTt1UQRk1Qh+zzbpkQ25RR9FvuEsvPQ								
GqwDWmed2TbnQ1tDbTBGUtT74ZTIGty35lPubvAE13dbBI2aVWGKjJtfRxDhpBQf							
zyZzl4iKx39MHTRoJVhnJ5eNVJv1BYMB+7FDg+zIgDAOj7u5eQtWnxDkWZH7FA+O								
FUhqTtXOl5Juw2eK6S/WG/lYlpOyVJibenvyCsqOHs1LIYS0kxMM2z6ZgEEtbe58								
xgVNPqECgYEA3sJU3xSdKb47IE/b0fJzDhYjmk7gSyLk/bqyE//5bK79A6MXA9yz								
JjJwDgMI+odb9FrHCR2UIGzsyIXqTx9B0bF2e2sqaPUbra7a/+7O6I33i9vipjZU							
hzAWU1kV9zVzmC1U0+lFtviynMypz65XFVv5n8EeZ934C6dFjuyegZMCgYEAzMvW					
S0L3MCTGInFN/j/JgNb873ZWrVnz+GOgQFgAQmjUcyJOrEifgOrU+ZYhHEeU/gwD							
ZEPOTyfXAOJBkxz+JSUFe5xAkyygL/51woaVEkDGyeQiaSfnEckJ3u2xxUurxWiQ								
ycJuOxetwnVSxOOy8Kr0qmk75vbOkR9wyUKPKgkCgYAluM7agAklOnuUuzFEWkQ1							
jHY2+UhuMNiKRwVE8cHxL6jU5tdM5iDIRR5IoSbyFd3ygTTXTFT7MLbgNh05jNd+							
hQjFWZ5y657mSIf5cx1CsFfNLU0yTF0AD5qYPqvDkx+iE3sb75LIq1DD0Lyo2KMS								
kOKytOdLO4F3p7nVvCgTVQKBgCMGGTf10+Bf6aKqTfRVZFisa8VoL5ql75tjLlzS							
r/irhOnLzDiakuyxPIsSqcb0Vv67fzj+f6H55kM4bo6CPtSLaEyjhEenMh4DHpCO						
A6CDg3uzkE77jAD2qMF/VQ+wyUeRgnF+1us0OXswJV+WsVuHYSBjruLpApq/DcLd								
py5BAoGAYqPrT05A522n3cuFgVCJKKeOBVv3WZQ0pscOic5fvN551IOiQ3S3TsGv							
ex4pTcxTvHRquF0RspeL4s9v//1DQibDvrmvVpU9iYEJ9PV9Lv5+XRNK0Kzlv6uk							
HE0hv/m0hKjUubCb8sCFe1vIum34I3dtWQB0YChQjp8F7rFP5oA=									
-----END RSA PRIVATE KEY-----			
***Grader Password								

   unix	
***OUTPUT:								

CLICK HERE http://13.232.39.180.xip.io/ 									
###### Reference used:																		
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
 

