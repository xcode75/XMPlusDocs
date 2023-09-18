# Use aaPanel to manually deploy

### 1. Configure aaPanel

You need to select your system in aaPanel to get the installation method. Here, CentOS 7+ is used as the system environment for installation.

Please be sure to use CentOS 7+ to install aaPanel, other systems may have unknown issues.

```
yum install -y wget && wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh && bash install.sh
```
After the installation is complete, we log in to aaPanel to install the environment.

Select the environment installation method using LNMP to check the following information

☑️ Nginx
☑️ MySQL
☑️ PHP 7.4
☑️ phpMyAdmin

Choose Fast to compile and install.

### 2. Install Ioncube 、fileinfo
aaPanel  > App Store > PHP 7.4 > Setting > Install extentions > Ioncube, fileinfo。

### 3.Delete disabled functions
aaPanel  > App Store > PHP 7.4 > Setting > Disabled functions > remove from list (exec, system, putenv, proc_open)。

### 4. Add Website
aaPanel  > Website > Add site。
- Fill in the domain name you point to the server in Domain
- Select MySQL in Database
- Select PHP-74 in PHP Verison

### 5. Install XMPlus
After logging in to the server through SSH, visit the site path: cd /www/wwwroot/tld.com

The following commands need to be executed in the site directory.

### Delete the files in the directory
```
chattr -i .user.ini

rm -rf .user.ini .htaccess 404.html index.html
```

### Execute the command to install XMPlus
```
cd /www/wwwroot/tld.com

wget https://github.com/xcode75/XManagerPlus/releases/download/v20230718/XMPlus.zip

unzip XMPlus.zip

chmod +x install.sh

./install.sh

mv .user.ini /www/wwwroot/tld.com/public

rm -rf install.sh XMPlus.zip
```

aaPanel  > Databases > Root password

Login to phpMyAdmin with username root and your database root password

Server connection collation: utf8mb4_unicode_ci

Create a new database with: utf8mb4_unicode_ci 

- Import database.sql with phpMyAdmin.

  - file path  /www/wwwroot/tld.com/database/database.sql  
  
  - NB: Do not use the aapanal panel to import sql
  

- Edit configuration /www/wwwroot/tld.com/config/config.php 

  -  You can change admin login path to your preferred path. Must start with /
  
  - fill in your database information(use root as username and root password in config.php)

### 6.Configure site directory and pseudo-static

Edit the added site > Conf > Site directory > Running directory choose /public and save。

After the addition is complete, edit the added site > URL rewrite to fill in the pseudo-static information.

```
location / {
    try_files $uri /index.php$is_args$args;
}
```

### 7. Add SSL to website

Edit the added site  > Conf > SSL 

Check site domain and apply for certificate, Enable Force HTTPS

### Restart nginx

#### Create an administrator account:  

After logging in to the server through SSH, visit the site path:cd  /www/wwwroot/tld.com

input the command: php xmplus admin

#### Download application 

input the command: php xmplus download

