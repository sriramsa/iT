Installing Codiad - Web Editor
==============================

#### 1. Install Apache2 web server
```
pkg-name: apache2
```
Verify by accessing your machine using a browser

#### 2. Install PHP5
```
pkg-name: php5
```

#### 3. Install GIT
```
pkg-name: git
```

#### 4. Clone the source for Codiad locally
```
sudo git clone https://github.com/Codiad/Codiad /var/www/html/codiad
```

#### 5. Create an empty config.php in /var/www/html/config.php
```
sudo touch /var/www/html/codiad/config.php
```

#### 6. Make the folder writable by apache
```
sudo chown www-data:www-data -R /var/www/html/codiad
```

#### 7. Configure Codiad
In a browser, go to 
```
http://<RPi-Host-Name>/codiad/
```
You should see "Initial Setup" screen. 
Create a new account and project name.

