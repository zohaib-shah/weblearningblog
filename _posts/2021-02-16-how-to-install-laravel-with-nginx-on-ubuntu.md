---
layout: post
title: "How to install Laravel with NGINX on Ubuntu"
date: 2021-02-16
categories: [Laravel]
tags: [laravel]
---

Laravel is the most popular PHP MVC framework.  If you have chosen a VPS or cloud hosting to deploy your laravel website, this post will help you install Laravel with NGINX on Ubuntu.


*PHP 7.4 Ubuntu 20.04 Laravel 8.x*


### PHP and PHP-FPM


PHP-FPM is an advance level FastCGI process manager. More information on PHP-FPM is here. To install laravel with NGINX on Ubuntu, you must have PHP-FPM installed.


Open Ubuntu terminal and run following commands:


sudo apt update
sudo apt install php php-fpm


This should install latest version of PHP and PHP-FPM. You can verify the same by using `php -version`. The status of PHP-FPM can be checked with `systemctl status php7.4-fpm`


### Installing common PHP modules and Laravel dependencies


Let's find some commonly used PHP modules. I found information at this link useful. Let's install all these common modules.

sudo apt install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl


Now, we need to make sure that all the PHP modules needed for Laravel (8.x) to work are also installed.


sudo apt install php7.4-bcmath openssl php7.4-json php7.4-mbstring


Verify that all the modules are installed by running `php -m`


### Setting up Composer on Ubuntu


As NPM is for NodeJS and PIP is for Python, PHP has it's dependency manager in the shape of [Composer](https://getcomposer.org" target="_blank" rel="noreferrer noopener nofollow). Install Composer on Ubuntu with following simple steps:

```bash
sudo apt install curl
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

### Add Laravel installer through composer


Let's add laravel installer through composer.

```bash
composer global require laravel/installer
```

Laravel installer is now available to use, but possibly Ubuntu shell doesn't know the directory to execute laravel installer from.


To make Ubuntu recognize laravel installer, add composer's system-wide vendor bin directory to the PATH variable.

```bash
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
```

It is important to note here that the location of composer's vendor bin directory is different for each operating system (My one is Ubuntu 20.04)


After setting the PATH variable, we can now run `laravel new first-laravel-app`. Here, **first-laravel-app** is the name of the project and could be anything of your choice.


cd into **first-laravel-app** and run `php artisan serve`.


We can now see the laravel development server running at http://127.0.0.1:8000/. Go ahead and check if all looks good. The purpose of this post is not to show how a development server will run laravel app but how to install laravel with NGINX on Ubuntu.


### Install NGINX on Ubuntu


NGINX is a powerful web server with load balancing support. NGINX serves PHP through PHP-FPM. As we have already setup PHP and PHP-FPM, let's go ahead and install NGINX on Ubuntu.


sudo apt install nginx

Ideally, all the websites on the server will have it's own NGINX configuration in `/etc/nginx/conf.d` directory.


As we have a fresh install of NGINX on Ubuntu the conf.d directory is empty.


To serve laravel app through NGINX, let's create a simple NGINX conf file for laravel app inside `/etc/nginx/conf.d`. You can name this file anything but it should end with `.conf` extension. NGINX will automatically pick every file stored in `/etc/nginx/conf.d` directory. This is because of following line placed in `/etc/nginx/nginx.conf`

```nginx
include /etc/nginx/conf.d/*.conf;
```

I have picked the simple laravel nginx conf file from laravel's official deployment page.

```nginx
server {
    listen 80;
    server_name 127.0.0.1;
    root /home/zohaib/websites/first-laravel-app/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
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

The only line you need to change is `root /home/zohaib/websites/first-laravel-app/public;`


I am on localhost and therefore have server_name directive set to 127.0.0.1. Usually, the server_name directive should have your original domain name. The server_name directive tells nginx to use the current nginx file if the requested server name matches the server_name directive. server_name directive could look like below for live environment:

```nginx
server_name www.yourdomain.com yourdomain.com;
```

You can validate nginx configurations with `sudo nginx -t`. Make sure that NGINX and PHP-FPM services are running with `sudo systemctl status nginx` and `sudo systemctl status php7.4-fpm` respectively.


Now, open `127.0.0.1` in your browser and you should see Laravel welcome page. If you see any permissions error on storage directory, change the root directory permissions as below:

```bash
sudo chown -R www-data:www-data /home/zohaib/websites/first-laravel-app
```

### Conclusion


 We have seen how to install Laravel with NGINX on Ubuntu. The process should be straight forward for you. You might run into some permissions and PATH settings issues. In such cases, you should find appropriate settings for your variant of Ubuntu.


I have also written a beginner installation guide on windows [here](http://weblearningblog.com/django/how-to-install-django-on-windows/" target="_blank" rel="noreferrer noopener).


If you have liked this post, Kindly consider sharing it and subscribing to my blog.
