# Linux-Server-Configuration

```
if (isAwesome){
  return true
}
```
# Server Information

URL: http://52.41.130.236.xip.io/
Port: 2200
Public IP: 52.41.130.236

# Introduction

This is the project number 6 for Udacity's Full Stack Software Developer Nanodegree. 
The main goal of this project is to install a Linux server and prepare it to host a web application. 
The web application hosted can be found in [this] (https://github.com/anikolengyel/Udacity-Full-Stack-Item-Catalog) repository.
I installed and configured a database server and deployd my Item Catalog application onto it.

I installed the following packages and used the following softwares:

- Apache2
- mod_wsgi
- virtualenv
- requests
- oauth2client
- Flask
- Psycopg2
- httplib2
- SQLAlchemy
- PostgreSQL
- Amazon Lightsail

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


