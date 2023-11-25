# HOW TO DEPLOY A WEBSITE ON UBUNTU WITH APACHE2

## Domain And Hosting
- Domain: take a domain name that you want for your website, there are a lot of suppliers you can choose
- Hosting: a lot of choices for you, I recommend an Ubuntu server because I write this doc for Ubuntu
- Point your domain name to your server ip: just create an A record from the supplier domain name management

## Installing Apache
```
sudo apt update
sudo apt install apache2
```
- Apache comes with a basic site, its content in /var/www/html
- To test apache2 works or not: http://127.0.0.1/ or http://localhost/ or http://Your-LAN-IP>/ or http://Your-Public-IP/

## Deploy Your Website
- Put your website folder in the /var/www/ directory (make sure you have an index.html in this folder)
- Make your own config file:
  + cd /etc/apache2/sites-available/
  + sudo cp 000-default.conf your-site-name.conf
- Edit your configuration:
  ```
  sudo nano your-site-name.conf
  ```
  ```
  <VirtualHost *:80>
	#...
	ServerAdmin your-email
	ServerName your-domain-name # usually start with: www.abc.com
	DocumentRoot /var/www/your-website-directory>
	#...
  </VirtualHost>
  ```
- Activate your website:
  ```
  sudo a2ensite <your-site-name>.conf
  service apache2 reload
  ```

## Add SSL
#### Copy Requirement files to Ubuntu (store anywhere what you want to)
- private.key (the key was generated to register SSL)
- RootCA.crt: this file received from the SSL supplier
- Domain.crt: this file received from the SSL supplier (Sectigo is *.pem file)
#### Turm on Apache2 SSL:
- Turn SSL module: ``` sudo a2enmod ssl ```
- Restart apache2: ``` sudo service apache2 restart ```
#### Edit Domain.conf file
  + Open your-domain.conf file in the /etc/apache2/sites-enabled/ directory
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
#### Restart Apache2
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
