# WordPress Installation on Ubuntu

This guide provides step-by-step instructions for setting up WordPress on an Ubuntu server.

## Prerequisites
- Ubuntu server
- SSH access with sudo privileges
- A registered domain name (optional but recommended)

## Step 1: Update System Packages
```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install Apache Web Server
```bash
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo ufw allow in "Apache Full"
```

## Step 3: Install MySQL/MariaDB
```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

### Create WordPress Database
```bash
sudo mysql
```
```sql
CREATE DATABASE wordpress_db;
CREATE USER 'wordpress_user'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Step 4: Install PHP and Required Extensions
```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip unzip -y
sudo systemctl restart apache2
```

## Step 5: Download and Configure WordPress
```bash
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
sudo mv wordpress /var/www/html/
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
```

### Configure WordPress
```bash
cd /var/www/html/wordpress
cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```
Modify the following lines:
```php
define('DB_NAME', 'wordpress_db');
define('DB_USER', 'wordpress_user');
define('DB_PASSWORD', 'your_secure_password');
define('DB_HOST', 'localhost');
```
Save and exit.

## Step 6: Configure Apache for WordPress
```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```
Add the following:
```
<VirtualHost *:80>
    ServerAdmin admin@your-domain.com
    DocumentRoot /var/www/html/wordpress
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    <Directory /var/www/html/wordpress/>
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Save and exit.

Enable the site and restart Apache:
```bash
sudo a2ensite wordpress
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## Step 7: Secure WordPress with SSL (Optional)
```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache
```

## Step 8: Complete Installation via Browser
1. Open `http://your-domain.com` or `http://your-server-ip`
2. Follow the WordPress setup wizard to create an admin account.

## Conclusion
Your WordPress site is now successfully installed and running on Ubuntu! 🚀

✅ Alternative: Define FS_METHOD in wp-config.php
If the issue persists, add this line to your wp-config.php file:

bash
Copy
Edit
sudo nano /var/www/html/wp-config.php
Add this line above /* That's all, stop editing! Happy publishing. */:

php
Copy
Edit
define('FS_METHOD', 'direct');

✅ Fix: Change Ownership and Permissions
1️⃣ Give the Web Server Ownership of WordPress Files
Run this command to set the correct owner (for Apache or Nginx):

bash
Copy
Edit
sudo chown -R www-data:www-data /var/www/html/
2️⃣ Set Correct File and Folder Permissions

bash
Copy
Edit
sudo find /var/www/html/ -type d -exec chmod 755 {} \;  # Directories
sudo find /var/www/html/ -type f -exec chmod 644 {} \;  # Files
3️⃣ Give Write Permissions to WordPress Uploads & Upgrade Folders

bash
Copy
Edit
sudo chmod -R 775 /var/www/html/wp-content/uploads
sudo chmod -R 775 /var/www/html/wp-content/upgrade
4️⃣ Restart the Web Server rr
For Apache:

bash
Copy
Edit
sudo systemctl restart apache2
