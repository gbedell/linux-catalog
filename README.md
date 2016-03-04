# linux-catalog

IP Address: 52.27.247.145

SSH Port: 2200

URL: http://ec2-52-27-247-145.us-west-2.compute.amazonaws.com/

Software Installed:
- apache2
- git
- python-pip
- virtualenv
- Flask
- postgresql
- postgresql-contrib
- python-sqlalchemy
- python-psycopg2
- python-oauth2client
- fail2ban
- glances

# Steps Taken to Configure Server

## 1. SSH Into Server
```
ssh -i ~/.ssh/udacity_key.rsa root@52.27.247.145
```

## 2. Create User grader
```
adduser grader
```

## 3. Sudo Permissions
```
visudo
```

## 4. Update Currently Installed Packages
```
sudo apt-get update
sudo apt-get upgrade
```

## 5. Change SSH Port
Edit the configuration file using:
```
sudo nano /etc/ssh/sshd_config
```

Change Port to 2200, Save and Quit.  Reload SSH using:
```
sudo service ssh restart
```

## 6. Configure Uncomplicated Firewall (UFW)
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200
sudo ufw allow 80
sudo ufw allow 123
sudo ufw enable
```

## 7. Configure the local timezone to UTC
```
sudo dpkg-reconfigure tzdata
```

## 8. Install and Configure Apache to serve a Python mod_wsgi application
```
sudo apt-get install apache2
```
Follow instructions found here - https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

When you reach Step 2 of the instructions to create application directory structure, use these instructions instead:
```
sudo apt-get install git
cd /var/www/
sudo mkdir catalog
cd catalog
sudo git clone https://github.com/gbedell/catalog.git
cd catalog
sudo rm README.md catalog.db database_setup.pyc delete_all_entries.py
```
As you are following the rest of the instructions, simply replace "FlaskApp" with "catalog"

## 9. Install and Configure PostgreSQL
Follow instructions found here - https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04

To create user catalog:
```
createuser catalog
psql
ALTER USER catalog WITH PASSWORD 'xxxxxx';
```

Change database connection in __init__.py and database_setup.py to:
```
('postgresql://catalog:catalogpassword@localhost/catalog')
```
