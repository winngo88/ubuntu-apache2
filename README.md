# HOW TO DEPLOY A WEBSITE ON UBUNTU WITH APACHE2

## Setup Apache2
  - To do ...
## Deploy Your Site
  - To do ...

## Add SSL
### Copy Requirement files to Ubuntu (store anywhere what you want to)
- private.key (copy this private key from email you have received from matbao, crete a new file *.key file)
- RootCA.crt: this file received from the SSL supplier
- Domain.crt: this file received from the SSL supplier (Sectigo is *.pem file)
### Turm on Apache2 SSL:
- sudo run command: sudo a2enmod ssl
- sudo service apache2 restart
### Edit Domain.conf file
  + Open <your domain>.conf in the /etc/apache2/sites-enabled/ directory
  + Edit follow:
    ```
	<VirtualHost *:443>
        ServerAdmin <your email>
        ServerName <your domain name> # usually start with: www.abc.com
        DocumentRoot /var/www/<your website directory>

        SSLEngine on
        SSLCertificateFile <your path>/Domain.crt
        SSLCertificateKeyFile <your path>/private.key
        SSLCACertificateFile <your path>/RootCA.crt
	</VirtualHost>
    ```
### Restart Apache2
- Check syntax:
``` sudo apachectl configtest ```
- Restart:
``` sudo service apache2 restart ```
## Redirect HTTP To HTTPS
  ```
  NameVirtualHost *:80
	<VirtualHost *:80>
	   ServerName <your domain name>
	   Redirect permanent / https://<your domain name>
	</VirtualHost>
	<VirtualHost _default_:443>
        ServerAdmin <your email>
        ServerName <your domain name> # usually start with: www.abc.com
        DocumentRoot /var/www/<your website directory>

        SSLEngine on
        SSLCertificateFile <your path>/Domain.crt
        SSLCertificateKeyFile <your path>/private.key
        SSLCACertificateFile <your path>/RootCA.crt
	</VirtualHost>
  ```
