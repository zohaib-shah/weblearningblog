---
layout: post
title: "How to Install Laravel on Windows Xampp"
date: 2018-04-26
categories: [Laravel]
tags: [laravel]
---

# How to Install Laravel on Windows Xampp
In this post, we will learn how to install laravel on windows xampp. Laravel is a popular PHP framework and as it is based on PHP, we need to have PHP configured on our web server. By web server, I mean any computer with Apache, PHP and MySQL installed.


XAMPP is a package which consists of Apache, MySQL and PHP. If we install XAMPP on our PC, we don't need to install Apache, PHP and MySQL separately, which sometime becomes a complicated task to execute. If you don't yet have installed XAMPP, go ahead and install it from this link.


Current version of laravel .i.e. 5.6 requires PHP 7.1.3 to work. Make sure that PHP version of your XAMPP installation meets this requirement.


To install laravel on windows xampp, we will follow these steps:


## Install Composer


Laravel relies on composer to manage it's dependencies. Using composer encourages us to use command line extensively which looks difficult at first but once we get familiar, it accelerates the development process.


Go to this link, click download button and install composer on your computer.


## Run composer command to install laravel on windows xampp


Navigate to the **htdocs** folder of xampp in windows command line.


If you just want to install latest version of laravel run this command:

`composer create-project --prefer-dist laravel/laravel lapp`

If you need to install an older version run this command:

`composer create-project --prefer-dist laravel/laravel lapp 5.4`

Notice the version appended to the command. Also notice that "lapp" here is the name of your application, you can name it whatever you want.


All the dependencies and packages would be downloaded and installed one by one. After completing the installation, you will see the following screen:


Now you can browse the **htdocs** directory, you will see that a folder named "lapp" has been created with all the files and directories of Laravel.


You can also open localhost link .i.e "http://localhost/lapp/public" on your browser to see the welcome screen of laravel installation on windows xampp.


## Conclusion


In this post, we have learned how to install laravel on windows xampp environment. In upcoming post we will learn how to remove public from laravel URL.
