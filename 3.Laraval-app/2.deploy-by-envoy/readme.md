
# 1. Deploying Laravel using Envoy


### System Requirements
To be able to run Laravel Boilerplate you have to meet the following requirements:
- PHP >= 7.4
- PHP Extensions: BCMath, Ctype, Fileinfo, JSON, Mbstring, OpenSSL, PDO, Tokenizer, XML, cURL, Mcrypt, GD
- Node.js >= 8.x
- Composer >= 1.9.x


### 1.Install Composer & nvm  [here](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)

- Composer install

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
- nvm install
```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

nvm -v
nvm install node 
node -v
npm -v
```

## 2.Local project setup

```
ssh -V
git clone https://github.com/Labs64/laravel-boilerplate.git dploy
cd dploy
sudo rm -rf .git
cp .env.example .env
composer install --prefer-dist
php artisan key:generate
npm install
npm run dev
php artisan migrate --seed
php artisan serve

```

## 3.LEMP Sever creation and firewall setup

```
adduser mark
usermod -aG sudo mark
rsync --archive --chown=mark:mark ~/.ssh /home/mark
sudo apt install nginx -y
sudo apt install mysql-server -y
sudo mysql_secure_installation
```
 - tips (all y)


```

SELECT user , authentication_string, plugin, host FROM mysql.user;
CREATE USER 'laravel'@'localhost' IDENTIFIED BY '@Ausa4422' ;
CREATE DATABASE laravel_admin;
GRANT ALL PRIVILEGES ON laravel_admin.* TO 'laravel'@'localhost';
exit;

mysql -u laravel -p
SHOW DATABASES;
exit;
```
- php fpm install


```
sudo apt install php-fpm php-mysql php-dom php-mbstring php-cli php-zip wget unzip php-curl  -y
php -v

```
- ufw install and setup
```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
sudo ufw enable
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

```
- sudo vim /etc/nginx/sites-enabled/defult

```
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /var/www/html;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
```
cd /var/www/html
ls
sudo rm index.nginx-debian.html
sudo vim index.php
```

```
<?php
phpinfo();
```
```
sudo nginx -t
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl restart nginx
```
## 5.GIT repo setup and Nginx configuration

- create new repo on github
- push code to new repo from local pc

```
adduser deploy
usermod -aG sudo deploy
```
- ps aux|grep nginx
```
sudo usermod -aG www-data $USER
cd /etc/nginx/sites-available
ls -la
```
- #############
```
sudo unlink /etc/nginx/sites-enable/default
sudo mv default laravel.devopshub.cf.conf
sudo ln -s /etc/nginx/sites-available/laravel.devopshub.cf.conf /etc/nginx/sites-enabled/
```
- #############
```
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl restart nginx
```
## 6.Install Laravel project on server

```
cd /var/www/html
sudo mkdir laravel.devopshub.cf
sudo rm index.php
ls -la
sudo chown deploy:www-data laravel.devopshub.cf/
ls -la
```
- #############

```
cd
ssh-keygen -t rsa -b 4096 -C "deploy@http://laravel.devopshub.cf"
cd .ssh
cat id_rsa.pub 
```
- set id_rsa.pub to github
- git clone ...... yourdomain.com/

## 7.First project build on the live server

- cp .env.exmple .env
(edit env file)
```
php artisan config:cache
```

- Composer install
```
cd
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer

composer install --no-dev
```
- Note: (composer fail for 1gb ram lets create sawp memory)
- sawp memory Create commund
```
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1
```
- #############

```
composer install --no-dev
sudo apt install php-curl -y
composer install --no-dev
```

- nvm install
```
cd 
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

nvm -v
nvm install node 
node -v
npm -v
 ```
 - #############
 ```
cd /var/www/html/laravel.devopshub.cf
npm install --only=prod
npm run prod
```
## 8.Laravel folder permissions and config file

- again permition deny for ACL
```
sudo setfacl -Rm u:www-data:rwx, u:deploy:rwx
sudo setfacl storage/
sudo setfacl -Rdm u:www-data:rwx, u:deploy:rwx
```
### 9.Laravel production commands and migrations

- Again 500 eror for artsen key

```
php artisan key:generate
composer dump-autoload -o
php artisan route:cache
php artisan config:cache
php artisan migrate:fresh --seed
```

## 10.Laravel Envoy installation and setup 
## 11.Linux Interactive shell potential pitfalls
## 12.Laravel Envoy deployment script
## 13.Creating Laravel Envoy stories






