### Linux Server Configuration Project.


# Why this Project?

- A deep understanding of exactly what your web applications are doing, how they are hosted,
- and the interactions between multiple systems are what define you as a Full Stack Web Developer.
- In this project, you’ll be responsible for turning a brand-new, bare bones,
- Linux server into the secure and efficient web application host your applications need .

 
* IP Address: 18.191.254.175
* SSH Port:   2200
* URL using DNS: http://arabsuccess.org




### The Steps Followed to Configure the server .


1. Update all packages.

* sudo apt-get update 
* sudo apt-get upgrade 




2. Configure the local timezone to UTC.

* sudo timedatectl set-timezone UTC 



3. Create a new user grader and Give him sudo access.

* sudo adduser grader 
* sudo nano /etc/sudoers.d/grader 
- Then add the following text ( grader ALL=(ALL) NOPASSWD:ALL ) 



4. Setup SSH keys for grader.

### On local machine WRITE (ssh-keygen) Then choose the path for storing public and private keys.
### then activate user and run this commands .

* sudo su - grader
* mkdir .ssh
* touch .ssh/authorized_keys 
* sudo chmod 700 .ssh
* sudo chmod 600 .ssh/authorized_keys 
* nano .ssh/authorized_keys 

### Then paste the contents of the public key created on the local machine and save file .
### for get the ssh_key u can write ( sudo cat /the path ssh_key )



5. Change the SSH port from 22 to 2200 .

* sudo nano /etc/ssh/sshd_config

### Then change the following:

* Find the Port 22 and transfer to (2200)
* Find the PasswordAuthentication line and edit it to (no)
* Find the PermitRootLogin line and edit it to (no)

# Then Save the file and run (sudo service ssh restart)




6. Configure the Uncomplicated Firewall (UFW)

* sudo ufw default deny incoming
* sudo ufw default allow outgoing
* sudo ufw allow 2200/tcp
* sudo ufw allow www
* sudo ufw allow ntp
* sudo ufw allow 8000/tcp
* sudo ufw enable




7. Install Apache2 and mod-wsgi for python3 and git.

* sudo apt-get install apache2 libapache2-mod-wsgi-py3 git





8. Install and configure PostgreSQL .

* sudo apt-get install libpq-dev python3-dev
* sudo apt-get install postgresql postgresql-contrib
* sudo su - postgres
* psql

Then

* CREATE USER catalog WITH PASSWORD 'password';
* CREATE DATABASE catalog WITH OWNER catalog;
* \c catalog
* REVOKE ALL ON SCHEMA public FROM public;
* GRANT ALL ON SCHEMA public TO catalog;
* \q
* exit

- Notes / you can change 'password' to your password 
- Notes / must be change the line database old to data the new database (engine = create_engine('postgresql://catalog:password@localhost/catalog') in all files have.




9. Clone the Catalog app from GitHub and Configure .

* cd /var/www/
* sudo mkdir catalog
* sudo chown grader:grader catalog
* git clone <your_repo_url> 
* cd catalog
* sudo nano catalog.wsgi


### Then add the following in catalog.wsgi file.

#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")

from app import app as application
application.secret_key = 'Super_Secret_key'





10. Configure apache server .

* sudo nano /etc/apache2/sites-enabled/000-default.conf

- Then add this data.

<VirtualHost *:80>
  ServerName arabsuccess.org
  ServerAdmin <Email>
  DocumentRoot /var/www/catalog
  WSGIDaemonProcess catalog user=grader group=grader
  WSGIScriptAlias / /var/www/catalog/catalog.wsgi

  <Directory /var/www/catalog>
    WSGIProcessGroup catalog
    WSGIApplicationGroup %{GLOBAL}
    Require all granted
  </Directory>

</VirtualHost>


# notes / must be change the server name to ip or your domin if you have domin name .



11. Must be install all this libraries for work application .

- sudo apt-get -y install python3-pip
- sudo apt-get -y install python-pip
- pip3 install flask packaging oauth2client redis passlib flask-httpauth
- pip3 install sqlalchemy flask-sqlalchemy psycopg2 bleach requests
- sudo pip3 install -t lib google-auth google-auth-httplib2 google-api-python-client --upgrade
- sudo pip3 install --upgrade google-api-python-client
- pip3 install --upgrade oauth2clien
- sudo pip install requests
- pip3 install flask packaging oauth2client redis passlib flask-httpauth
- pip3 install sqlalchemy flask-sqlalchemy psycopg2 bleach requests
- sudo apt-get install python-httplib2
- pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib
- pip install oauth2client
- sudo pip install flask_sqlalchemy
- sudo pip install sqlalchemy
- pip install psycopg2-binary
- pip install psycopg2
- pip install google




12. for see all errors and resolve in the server after installation you can used this command.



* sudo cat /var/log/apache2/error.log 




13. must be make restart and reload the server after resolve any problem.

* sudo service apache2 reload
* sudo service apache2 restart







### Important Information .

* The WSGI (Web Server Gateway Interface) is an interface between web servers and web apps for python. 
* Mod_wsgi is an Apache HTTP server mod that enables Apache to serve Flask applications.





### for connect to ssh from termianl must be follow this steps because your cant access after active the filrewall .

1. Downlaod the key pair have extend .pem
2. open the terminal from folder downloaded the key
3. chmod 400 (add the key name)
4. ssh -i (the_key_name_.pem) ubuntu@ip 
5. change the port 22 to 2200 and PasswordAuthentication and permitRootLogin same line (5) above
6 go to instance and inside network then add protocol 2200 
7. exit
8. ssh -i (your_key.pem) -p 2200 ubuntu@ip
9. make firewall setting same line(6) above


### the link repository readmefile (https://github.com/apomohab/readmelinux)



### for configuration your domin for accesss at the ip you can see this video and follow steps.

- https://www.youtube.com/watch?v=fkP94Iuyb3Q&feature=youtu.be



### Good Luck ...













