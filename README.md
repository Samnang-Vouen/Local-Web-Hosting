# Local Website Hosting with HTTPS & Auto-Redirect (Ubuntu + Apache)

## **🚀 Objective**
This project demonstrates how to host a website locally on Ubuntu using Apache, enable HTTPS, set up auto-redirect from HTTP to HTTPS, and configure it as a reboot-persistent service. The website is accessible via a local domain name (`segroup2.com.cadt..`).

## **🔧 Prerequisites**
- Ubuntu (or any Linux-based OS).
- Apache installed.
- Git installed.
- SSL certificate generation.

## **📋 Steps**

### **1️⃣ Install Apache & Required Packages**
First, update your system and install Apache and OpenSSL:
```
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 openssl -y
```
Enable Apache to start on boot:
```
sudo systemctl enable apache2
sudo systemctl start apache2

```
### **2️⃣ Clone the Website Repository
Clone the website repository into the Apache web root directory:
```
sudo git clone https://github.com/ivysone/Will-you-be-my-Valentine- /var/www/segroup2
```
Set proper permissions:
```
sudo chown -R www-data:www-data /var/www/segroup2
sudo chmod -R 755 /var/www/segroup2
```
### **3️⃣ Configure Local Domain Name (segroup2.com.cadt..)
Edit `/etc/hosts` on your Ubuntu server and all client machines that need access to the local website:
```
sudo nano /etc/hosts
```
Add the following line (replace 192.168.x.x with your server's local IP):
```
192.168.x.x  segroup2.com.cadt..
```
Save and exit (CTRL+X, Y, Enter).
### **4️⃣ Create Apache Virtual Host Configuration
Create a new Apache virtual host configuration file:
```
sudo nano /etc/apache2/sites-available/segroup2.conf
```
Add the following configuration:
```
<VirtualHost *:80>
    ServerName segroup2.com.cadt..
    DocumentRoot /var/www/segroup2

    ErrorLog ${APACHE_LOG_DIR}/segroup2_error.log
    CustomLog ${APACHE_LOG_DIR}/segroup2_access.log combined

    RewriteEngine on
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</VirtualHost>

<VirtualHost *:443>
    ServerName segroup2.com.cadt..
    DocumentRoot /var/www/segroup2

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/segroup2.crt
    SSLCertificateKeyFile /etc/ssl/private/segroup2.key

    <Directory /var/www/segroup2>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/segroup2_ssl_error.log
    CustomLog ${APACHE_LOG_DIR}/segroup2_ssl_access.log combined
</VirtualHost>
```
Save and exit.

Enable the site:
```
sudo a2ensite segroup2.conf
```
Enable the rewrite and ssl modules:
```
sudo a2enmod rewrite ssl
```
## **5️⃣ Generate a Self-Signed SSL Certificate
Generate a self-signed SSL certificate:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
 -keyout /etc/ssl/private/segroup2.key -out /etc/ssl/certs/segroup2.crt
```
Complete the details in the prompt. Set `Common Name` to `segroup2.com.cadt`
Restart Apache to apply the changes:
```
sudo systemctl restart apache2
```
## **6️⃣ Make It a Reboot-Persistent Service
Create a systemd service file to ensure the website starts automatically after a reboot:
```
sudo nano /etc/systemd/system/segroup2.service
```
Add the following configuration:
```
[Unit]
Description=Apache Local Web Hosting Service
After=network.target

[Service]
ExecStart=/usr/sbin/apachectl -DFOREGROUND
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```
Save and exit.
Enable and start the service:
```
sudo systemctl enable segroup2
sudo systemctl start segroup2
```
## **7️⃣ Test Your Setup
Open a browser on a local machine.
Visit `http://segroup2.com.cadt` → It should automatically redirect to `https://segroup2.com.cadt`.

If you see SSL warnings, trust the certificate by adding it to the list of trusted certificates.
