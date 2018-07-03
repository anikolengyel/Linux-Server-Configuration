# Linux-Server-Configuration

# Server Information

URL: http://52.41.130.236.xip.io/

Port: 2200

Public IP: 52.41.130.236

# Introduction

This is the project number 6 for Udacity's Full Stack Software Developer Nanodegree. 
The main goal of this project is to install a Linux server and prepare it to host a web application. 
The web application hosted can be found in [this] (https://github.com/anikolengyel/Udacity-Full-Stack-Item-Catalog) repository.
I installed and configured a database server and deployd my Item Catalog application onto it.

This README contains an overall summary about the main steps to host my application on http://52.41.130.236.xip.io/.

# Configuration

__Create server instance__

We used [Amazon Lightsail] (https://aws.amazon.com/lightsail/) to set up a Linux server instance.

- Log in with AWS account into Lighstail
- Create an instance 
- Choose instance image - first choose OS Only, then Ubuntu for operating system
- Choose instance plan
- Give a hostname for your instance
- Start the instance then wait for the startup
- Connect to it using SSH in the web interface

__Server Configurations__

- Update and upgrade all packages to make the serer more secure

```
sudo apt-get update
sudo apt-get upgrade
```
- Change the SSH port from 22 to 2200

```
sudo vim /etc/ssh/sshd_config
```
Change the port from 22 to 2200 then restart SSH.

- Configure the UFW to allow incoming connections for SSH. Run the following commands:

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow www
sudo ufw allow 123/udp
sudo ufw allow 2200/tcp
sudo ufw deny 22
sudo ufw enable
```
- After that run ```sudo ufw status``` to check the opened ports and the ufw activity.

__Create User grader__

- Create a new user and name it grader.
```
sudo adduser grader
```
- give grader a permission to sudo.
```
sudo vim /etc/sudoers.d/grader
```
- Edit and add grader ```grader ALL=(ALL) NOPASSWD:ALL```.
- Create an SSH key par for grader. Run ```ssh-keygen``` on local machine and save the files.
- In the virtual machine, create a new directioy .ssh and the copy the content of the epub file
from the local machine into a new file (authorized_keys).
- Run ```chmod 700 .ssh``` and ```chmod 644 .ssh/authorized_keys```.
- Log in as grader.

__Change Timezone to UTC__

- Configure local timezon to UTC.
- Run ```sudo dpkg-reconfigure tzdata```.
- Choose None of the above and click on UTC.

__Get the Item Catalog Project__

- Install git ```sudo apt-get install git```.
- Clone the project repository from Github.
- Copy the ```application.py``` file into ```__init__.py```.
- Edit the ```__init__.py``` file and write app.run().

__Configure Login with Google OAuth__

- On Google's OAuth client console change the Authorized JavaScript origins to http://52.41.130.236.xip.io.
- Change Authorized redirect URIs to http://52.41.130.236.xip.io.

__Installation__

Install the following packages with pip:
- Apache2
- mod_wsgi
- virtualenv
- requests
- oauth2client
- Flask
- Psycopg2
- httplib2
- SQLAlchemy

For the global environment with sudo install libpq-dev.

__Set up a virtual host__

Created an ItemCatalog.conf file in /etc/apache2/sites-available and wrote the following code:

```
<VirtualHost *:80>
                ServerName XX.XX.XX.XX
                ServerAdmin lengyelaniko93@gmail.com
                WSGIScriptAlias / /var/www/ItemCatalog/ItemCatalog.wsgi
                <Directory /var/www/ItemCatalog/>
                        Order allow,deny
                        Allow from all
                        Options -Indexes
                </Directory>
                Alias /static /var/www/ItemCatalog/static
                <Directory /var/www/ItemCatalog/static/>
                        Order allow,deny
                        Allow from all
                        Options -Indexes
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
__Configure PostgreSQL__

- As postgres user create and configure database.
- Change the engine in ```__init__.py``` and ```database_setup.py``` to ```postgresql://catalog:catalog@localhost/catalog```.

__Populate the database__

- Activate the virualenvironment and run ```data_creating.py```.
- Restart Apache.
