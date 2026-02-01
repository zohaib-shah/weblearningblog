---
layout: post
title: "Remove public from Laravel URL"
date: 2018-04-28
categories: [Laravel]
tags: [laravel]
---

By default, We browse laravel application by appending **public** on URL. If you are using shared hosting environment, sometimes it is difficult to configure your hosting server to point to the public directory. In such cases, it is necessary to remove public from laravel URL.

There are multiple ways to remove public from laravel URL. In this post, we will follow the procedure which involves modifying directory structure and change few paths in index.php file.

If you have not yet installed laravel, follow this post.

## Step 1: Create a new folder in root directory of application

We need a folder where we can move all the directories and files except public directory. Go ahead and create a folder in root of your application, you can name it whatever you want, I am just naming it as source folder.


## Step 2: Move all contents except the public folder

Do a select all in root directory and exclude public and source folders. Now press "CTRL + X" to cut all the selected contents. Open source folder and paste all the contents there.


source folder would look like the image below now:


## Step 3: Move contents of public directory to root directory

Now go to the root of the application and open public directory. Cut all the content and paste it in root directory.


Public directory is empty now, so go ahead and remove it.


Final directory structure of our application would look like below image:


## Step 4: Change paths in index.php file

In the final step, we will change some paths in index.php file, which is now available in root directory of our application.

Open index.php file and replace following two lines:
require __DIR__.'/../bootstrap/autoload.php';
$app = require_once __DIR__.'/../bootstrap/app.php';

with these lines:
require __DIR__.'/source/bootstrap/autoload.php';
$app = require_once __DIR__.'/source/bootstrap/app.php';

We have successfully removed public from laravel URL. Open your browser and navigate to your application root directory. You can see that welcome screen is now being served from root directory instead of public directory. Remember, You also need to run artisan commands from the folder you created, in this case it is "source".
