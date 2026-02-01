---
layout: post
title: "How to convert a bootstrap template to Angular"
date: 2022-12-13
categories: [Angular]
tags: [angular]
---

In this post, we will learn how to convert any bootstrap template to angular. "Clean Blog" on startbootstrap.com seems to be a good choice as long as we are keen to understand the process of converting a bootstrap template to angular. We can download the template from [this](https://startbootstrap.com/theme/clean-blog" target="_blank" rel="noopener) link.


## 1- Setting up Angular app


First of all, check if you have ng-cli installed. Run `ng version` to confirm the version of ng-cli.


Create a fresh angular app using `ng new bootstrap-template`. When prompted, select "N" for routing as we won't be working with routes in this post.

Also, select "CSS" as stylesheet format of your app. Once the angular app is created, we should see following files and directories created for us:

├── angular.json
├── karma.conf.js
├── node_modules
├── package.json
├── package-lock.json
├── README.md
├── src
│   ├── app
│   ├── assets
│   ├── environments
│   ├── favicon.ico
│   ├── index.html
│   ├── main.ts
│   ├── polyfills.ts
│   ├── styles.css
│   └── test.ts
├── tsconfig.app.json
├── tsconfig.json
└── tsconfig.spec.json


`index.html` inside `src` directory is the starting point of our angular app. The app-root element in index.html file gets replaced with the content of the app component. In fact, app component is the parent component of all the components we are going to create.


The app component is already available when we create a new angular project using ng-cli. The reason why app-root element is replaced with the rendered app component is the code below, note that the selector is 'app-root' in app.component.ts file.


import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'bootstrap-template';
}


## 2- Setting up Bootstrap library with Angular


Once we have the angular app setup, its time to add bootstrap libray in the app. Open terminal in project root and type `npm i --save bootstrap`, this will add bootstrap in `node_modules`. We need two files from bootstrap package node_modules/bootstrap/dist/css/bootstrap.min.css and node_modules/bootstrap/dist/js/bootstrap.min.js

We need to add both these files globally in our app. We can add it directly into `src/index.html` file or we have the option to add these files in `angular.json` file located at project root directory.


Given below, is the content of `angular.json` file:


{
  ....,
          "options": {
            "outputPath": "dist/bootstrap-template",
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "tsconfig.app.json",
            "assets": [
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css",
              "src/styles.css"
            ],
            "scripts": [
              "node_modules/bootstrap/dist/js/bootstrap.min.js"
            ]
          },
          ...
}


Here, we are only seeing `projects.architect.build.options` node of the complete JSON. Scripts and styles defined here becomes globally accessible in the app.


We have added bootstrap in our angular application, to confirm this open `app.component.html` file and remove all its content. Now, just add bootstrap Alert to this file.


  This is a primary alert—check it out!


Now run `npm start` to check if bootstrap Alert renders as expected.


## 3- Analyzing the downloaded bootstrap template


Download and extract the bootstrap template mentioned previously. Let's have a look at its content below:

├── about.html
├── assets
│------------ ├── favicon.ico
│------------ └── img
├── contact.html
├── css
│------------ └── styles.css
├── index.html
├── js
│------------ └── scripts.js
└── post.html


Open `index.html` file. It should look like as below:


Our aim here is to convert this page into its relevant angular representation. As components are an integral part of the angular app. We should first analyze this bootstrap template and think about the possibility of distributing this page into standalone components.


## 4- Converting the template to Angular


Let's start converting bootstrap template to angular. From the downloaded template, open index.html file in text editor and copy everything inside `body` tag except .js imports. Paste this inside /src/app/app.component.html file and run `npm start`. As we have already included bootstrap in angular app, running the app now shows something similar to the actual bootstrap template.


`/src/styles.css` is included throughout the angular app as mentioned in angular.json file. We will copy content of bootstrap template's `/css/styles.css` file and paste it in `/src/styles.css`. Also, copy template's `/assets/img` directory to angular app's `/src/assets/img` directory.


Finally, head over to the template's head section and pick all the externally referred fonts.


Place all these fonts in the head section of angular app's `/src/index.html`. Browsing `http://localhost:4200/` should show a completely identical page as the template's actual index.html page.


## 5- Creating Angular components


An angular component should be a portion of a webpage which is self-contained and which can exist in isolation. From the index.html page, we can think of header, footer and post as potential candidates of becoming a component.


Header


Post


Footer


Let's create these components using angular cli:


ng g component Header
ng g component Post
ng g component Footer


Once all the components are created, the /src/app directory should look as below:


Components inside app component


By default, all components will be placed inside app directory. Each component will have its own template, data binding and styles. `@Component` decorator in component.ts files contains attributes to configure the underlying component.


@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})


### 5.1- The Header Component


Go back to app.component.html file and cut the html of header section.


                Start Bootstrap

                    Menu


                        Home
                        About
                        Sample Post
                        Contact


                            Clean Blog
                            A Blog Theme by Start Bootstrap


Now, place this html in `/src/app/header/header.component.html` file. Doing so, we have extracted header's html from app.component.html and added it in Header component. As app component is like a parent component of all newly added components, we need to place the Header component's selector as a replacement of actual header's html.


................


### 5.2- The Post Component


Again, copy the post html from app.component.html to /src/app/post/post.component.html.


                            Man must explore, and this is exploration at its greatest
                            Problems look mighty small from 150 miles up


                            Posted by
                            [Start Bootstrap](#!)
                            on September 24, 2022


As we have a list of posts in the bootstrap template, we would like to iterate over some list of posts and render our post component for each iteration. For now, just keep one post and remove the rest of them from template. As done previously, replace the post html with the post component selector ``.


### 5.3- The Footer Component


Finally, identify footer html in app.component.html and place it in `/src/app/footer/footer.component.html`.


                Copyright © Your Website 2022


Adding the footer selector `` in `app.component.html`, we will complete the "angularization" of our bootstrap template. The content of the `app.component.html` file should look like as below.


                    Older Posts →


### 5.4- Adding some life to post component


We need to have a model class for 'Post'. We also need to have a list of these post objects available in our app component, In order to render the post component for each of the post.


Let's first create a directory `/src/models`. Create a model class 'Post' as follows:


export class Post {
    constructor(
        public title:string,
        public description:string,
        public createdAt:string,
        public author:string

    ){

    }
}


To learn more about typescript classes, read [this](http://weblearningblog.com/typescript/working-with-classes-in-typescript/" target="_blank" rel="noopener) post.


Now in our app.component.ts file, import the 'Post' model and create a list of its objects as mentioned below:


import { Component } from '@angular/core';
import { Post } from 'src/models/post.model';
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  public posts:Post[] = [
    {
      title: 'My First Post',
      description: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry\'s standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.',
      createdAt: 'Dec 12 01:37',
      author: 'John Doe'
    },
    {
      title: 'My Second Post',
      description: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry\'s standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.',
      createdAt: 'Dec 12 05:20',
      author: 'James'
    },
    {
      title: 'My Third Post',
      description: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry\'s standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.',
      createdAt: 'Dec 12 10:20',
      author: 'John Doe'
    }
  ];
  title = 'bootstrap-template';
}


We can see that post component relies on title,description,author and createdAt attributes. We would like to enable post component to take these attributes from the parent component. To enable an angular component to accept input from the parent component, angular provides us with `@Input()` decorator.


import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-post',
  templateUrl: './post.component.html',
  styleUrls: ['./post.component.css']
})
export class PostComponent implements OnInit {
  @Input() title:string = '';
  @Input() description:string = '';
  @Input() author:string = '';
  @Input() createdAt:string = '';
  constructor() { }

  ngOnInit(): void {
  }

}


Now, we can render the post component for each of our post object from app component as follows:


                    Older Posts →


The `*ngFor` directive is responsible of repeating the "Post" component for each post.


Running the app now should give you a perfectly rendered page similar to index.html page of downloaded bootstrap template.


This post should serve as step by step guide to convert a bootstrap template to angular. Github repository for this blog post is available [here](https://github.com/zohaib-shah/bootstrap-angular-conversion" target="_blank" rel="noopener).
