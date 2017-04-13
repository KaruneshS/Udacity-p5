<b>IP Address: 107.21.35.134<br>
SSH port: 2200<br>
Application URL: http://107.21.35.134/login
</b>

Steps followed for Udacity Linux server configuration project:<br/>
* Creating user grader & generating ssh key for login<br/>
	- Create user grader using "sudo adduser grader"<br/>
	- Add SSH key in authorized keys for grader<br/>
	- Give sudoer access: "sudo visudo" --> Add line "grader ALL=(ALL) NOPASSWD: ALL"<br/> 
* Disable root remote login
	- Change /etc/ssh/sshd_config to set "PermitRootLogin no"
	- Restart ssh service "sudo service sshd restart"
* Enforce key-based SSH authentication
	- Change /etc/ssh/sshd_config to set "PasswordAuthentication no"
	- Restart ssh service "sudo service sshd restart"
* Change SSH port to 2200 & only allow http(80), ntp(123)
	- Change /etc/ssh/sshd_config & locate "Port 20",change it to "Port 2200"
	- Restart ssh service "sudo service sshd restart"
	- Change Network firewall setting in Lightsail to allow Custom port 2200, ntp(123), http(80)
* Update all applications
	- Enter "sudo apt-get update" --> "sudo apt-get upgrade"
* Install Apache & allow through firewall (Refer: https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)
	- Install using "sudo apt-get update" --> "sudo apt-get install apache2"
	- Set Global ServerName to Suppress Syntax Warnings: "sudo nano /etc/apache2/apache2.conf" --> Add "ServerName <Domain/Public IP>" at the bottom of the file
	- Test using "sudo apache2ctl configtest" --> Output should be "Syntax OK"
	- Restart Apache "sudo systemctl restart apache2"
	- Adjust the Firewall to Allow Web Traffic: "sudo ufw app list" --> "sudo ufw app info 'Apache Full'" --> "sudo ufw allow in 'Apache Full'"
	- Test "http://your_server_IP_address" for Apache default webpage
* Install mod_wsgi
	- Install using "sudo apt-get install libapache2-mod-wsgi"
	- Modify the file /etc/apache2/sites-enabled/000-default.conf to configure Apache to handle requests using the WSGI module
	- Modify & add line in Virtual host configuration "WSGIScriptAlias / /var/www/html/myapp.wsgi"
	- Restart apache using "sudo apache2ctl restart"
	- Create & modify myapp.wsgi file using "sudo nano /var/www/html/myapp.wsgi" & add some sample program to test.

* Install Postgresql: (Refer: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04)
* Deploy Catalog app on the server: (Refer: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
	- Install Flask
	- Install sqlalchemy
	- Install oauth2client