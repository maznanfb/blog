---
title: "Deploy Laravel di Digitalocean"
date: 2018-10-05
tags: ["server", "laravel", "nginx", "php"]
draft: true
---

## Intro



## Setup Environment

OS: Ubuntu 18.04 x64

1. One-click Apps
2. pilih LEMP 18.04
3. Droplets: 2 GB 2 vCPUs 60 GB 3 TB
4. ssh key

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDA7X8YJQ4Dh8H+mgSRS9pmwXgoxhqH7K3toyRbobqQC9nU5OzqGQuFFg/UMCkqetyvWNNz/58RnBlm/jIQGNQtV6lkq41/T4M8Yanln1GVmpWEWI/D2S+pjBSt5h1hPhN0dGvZwQieWR19QQH4BCSmcr6Wmuy7t8dfUA3EkwSAGvyfxBhPcBGqAkJPCINNcE0cFl/LRMlIXR+ZSmkJ1S6xvSeo1bqmxECk0BB2OihTdGi10cY9wEAhx5EbodE0tQmf94O1u0/6sqoQ2DhAk7sBF89BqJh2Dx566i4JKmEpdxGd+rNb7eRpQw0TN4LR1csZvCsaWOyABZgx8Ez8+nWB

aznan@aznan-ThinkPad-X220

5. create

## initial setup
dari terminal:
ssh root@ip

adduser aznan
usermod -aG sudo aznan
{{< highlight go >}}
rsync --archive --chown=aznan:aznan ~/.ssh /home/aznan
{{< /highlight >}}

ufw allow OpenSSH
exit
login menggunakan ssh aznan@ip


## Configure Components
https://www.digitalocean.com/docs/one-clicks/lemp/
1. firewall 
sudo ufw allow 'Nginx HTTP'
2. Configure the PHP Processor
	sudo nano /etc/php/7.2/fpm/php.ini
	edit-> cgi.fix_pathinfo=0
	restart php-> sudo systemctl restart php7.2-fpm

3. Configure Nginx to Use the PHP Processor
sudo nano /etc/nginx/sites-available/digitalocean

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html/laravelproject/public;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.2-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}

sudo systemctl reload nginx

4. Setup Laravel Project
sudo mysql
CREATE DATABASE laravel DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
GRANT ALL ON laravel.* TO 'laraveluser'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
EXIT;

seeting up demo:
misal projeck:quickstart
masukkan projek ke /var/www/html/
sudo chown aznan:aznan /var/www/html/quickstart
composer install
sudo nano /var/www/html/quickstart/.env

DB_DATABASE=laravel
DB_USERNAME=laraveluser
DB_PASSWORD=password

php artisan migrate
composer install --optimize-autoloader --no-dev

permission:


sudo chown -R aznan:aznan storage bootstrap/cache public/files public/photos

sudo chgrp -R www-data storage bootstrap/cache public/files public/photos
sudo chmod -R ug+rwx storage bootstrap/cache public/files public/photos

sudo rm -r public/storage
php artisan storage:link

sudo apt-get install phpmyadmin
sudo ln -s /usr/share/phpmyadmin /var/www/html
sudo mv phpmyadmin wkwkwk

5. Securing your Application with TLS
harus memiliki domain
https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04

### thank you!