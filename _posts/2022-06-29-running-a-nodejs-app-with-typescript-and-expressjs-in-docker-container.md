---
layout: post
title: "Running a NodeJS app with Typescript and ExpressJS in Docker Container"
date: 2022-06-29
categories: [NodeJS]
tags: [javascript, typescript, nodejs, express, docker]
---

In this blog post, I will walk you through the simplest way of setting up a nodejs application with expressjs and typescript to run inside a docker container. Git repository for this post is available [here](https://github.com/zohaib-shah/express-ts-docker-starter" target="_blank" rel="noreferrer noopener nofollow).


We will complete the setup by following below steps:


Creating a simple NodeJS application with express.
Adding typescript support to express app
Containerize express app using Docker.


## 1. Creating a simple NodeJS project with express


Go ahead and create a new directory with `mkdir docker-express-ts-boilerplate && cd docker-express-ts-boilerplate`. Initialize the directory with new node project by `npm init -y`


Install express framework by running `npm i express --save`. Now create a file `app.js` with following contents:

```javascript
const express = require('express');
const app = express();
app.get('/',(req, res, next) => {
    res.send("Root route is working");
});
app.listen(3000,() => {
    console.log("Server started on port 3000");
});
```

Now open pacakge.json file and add the start script as shown below on line # 8:

```json
{
  "name": "docker-express-ts-boilerplate",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.1"
  }
}
```

Run `npm start` should start the server now on port 3000. Visit localhost:3000 to verify that the root route is responding.

You can follow [this](http://weblearningblog.com/nodejs/how-to-use-sequelize-with-node-and-express/" target="_blank" rel="noopener noreferrer) link for another post on this blog which explains setting up nodejs with mysql.


https://youtu.be/y-pQD-T3rQo


## 2. Adding typescript support to express app


### What is Typescript


Typescript allows us to write more structured code. Typescript converts our code into Javascript which can run on any browser or server based applications.


### Why do we need Typescript


As Javascript is a loosely typed language, there are chances that we unintentionally assign values to variables and nobody complains until the application behaves abnormally.


With Typescript, we define variables to use specific types. We use enums, classes and interfaces to make our code readable and modular.


### Setting Up Typescript with Node and Express


Install typescript as a development dependency by executing `npm i typescript -D`. We are installing it locally hence we need to add an npm script to use typescript.


After adding `tsc` as an script, package.json scripts part should look like below:


"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node app.js",
    "tsc": "tsc"
  },


Now, initialize typescript by running `npm run tsc -- --init`. This should create a `tsconfig.json` file in project root directory.


{
  "compilerOptions": {

    "target": "es2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    "rootDir": "./src",                                  /* Specify the root folder within your source files. */
    "outDir": "./build",                                   /* Specify an output folder for all emitted files. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */
    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}


A big chunk of configurations has been omitted from tsconfig.json file above. At this stage, we are only interested in `rootDir` and `outDir`. With rootDir, we can dictate `tsc` to pick all the .ts code from ./src directory and transpile it back to JS in `outDir`. Visit [https://aka.ms/tsconfig](https://aka.ms/tsconfig) to read more about this file.


We would like to install express types using `npm i @types/express -D`, this will help our application to be aware of the types available in express .e.g. Request, Response and NextFunction.


Go ahead and create a new directory `src` inside project root directory. This src directory will now have all our code. We will now transfer code written inside app.js earlier into `app.ts` file. This app.ts file lives inside src directory and it is now our actual project entry point.


import express, { Application, Request, Response, NextFunction } from 'express';
const app: Application = express();
app.get('/',(req: Request, res: Response, next: NextFunction) => {
    res.send("Root route is working");
});
app.listen(3000,() => {
    console.log("Server listening on port 3000");
});


To summarize this section, we can say that upon running `tsc`, Typescript will look for .ts files inside src directory and will convert all these .ts files into .js files to make browser or NodeJS understands our code. Typescript will place all these .js files in our `outDir` directory.


We will now change `start` script to pick app.js from `build` directory (as we configured in tsconfig.json file). We will also add a build script to run tsc.


"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node build/app.js",
    "tsc": "tsc",
    "build": "tsc"
  },


It should be noted here that whenever we update our code inside src directory. We should run `npm run build` to transpile .ts files into .js files. Afterwards, `npm run start` will execute our code from `build` directory.


## 3. Containerize express app with typescript using Docker


Now that we have our express application setup with typescript locally, we will use docker to containerize the app. It is assumed that you have both Docker Engine and Docker Compose installed already.


### Simple steps to containerize using Docker


In simple words, containerization takes place by implementing following steps:

Create a docker container from a node image.Create a working directory inside container. Working directory is simply a project root directory in docker container.Copy package.json file into the working directory.Run npm install to install all the npm dependencies listed in package.json file.Now copy remaining project source files to the working directory.Build the project inside container. Which will create a build directory in working directory.Expose container's port (3000 in this case) so that it is visible to host machine.


### The Dockerfile


To implement above steps in docker's way, create a `Dockerfile`. Please note that this file should not have any extension otherwise it will not work.

FROM node:latest
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm","start"]


### .dockerignore file


Create a .dockerignore file and following lines to it:

node_modules
build


The .dockerignore file will prevent unwanted files and directories to be copied to the container. In this case, we want that the `npm install` should run inside container and create fresh `node_modules` directory in container. We also want `build` to take place inside container.


### docker-compose.yml file for Node, Express and Typescript


Most likely, our nodejs application with typescript and docker
 will need more services. For example, database service, redis cache etc. Docker compose helps us build on top of a Dockerfile and add more services to the app. Also, each docker-compose service may spin up a new container.

version: '3.0'
services:
  app:
    container_name: app
    build: ./
    ports:
      - 3000:3000


The simplest possible docker-compose.yml file above will look for a Dockerfile in current directory. As we asked it to do so by `build: ./`. The ports directive is telling docker to map port 3000 of host machine to the port 3000 of the container. Port on the left side being the port of the host machine. Finally, run `docker-compose up` to build and run the container.


## Conclusion


I tried to show you the simplest way of setting up a nodejs application with expressjs and typescript to run in docker. In future posts, we will see how we can transform this setup to incorporate more features and project structure. Feel free to ask any questions in comments section below. Consider sharing this post if it has helped you in understanding the concept.

```