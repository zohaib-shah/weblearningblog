---
layout: post
title: "How to use Sequelize with Node and Express"
date: 2021-06-06
categories: [Node.js]
tags: [nodejs, express, sequelize]
---

# How to use Sequelize with Node and Express
If you are willing to use MySQL with your Node App, this guide will show you how to use Sequelize with Node and Express.


In my opinion, Sequilize ORM is the best way to use MySQL with Node. Sequelize offers features like Migrations, Seeders and Relationship management. These features are handy when you are in need of a quick working project structure.


### 1- Creating project structure using express-generator


Create a directory which will contain the application code. `mkdir express-app && cd express-app`


Inside the application directory, now run `npx express-generator`


The application directory will now look like this. This is a well known project structure for any express framework based web application.


While still in the root of project directory, run `npm install` to install all the packages listed in package.json file.


├── app.js

├── bin

├── node_modules

├── package.json

├── package-lock.json

├── public

├── routes

└── views


### 2- Installing Sequelize ORM


In order to communicate with MySQL database, The Sequelize ORM depends on mysql2 which is a mysql client for NodeJS.


Let's first install mysql2 by running `npm install --save mysql2`. Once the mysql client is installed, We can install **Sequelize** with `npm install --save sequelize`


### 3- Setting Up Sequelize-CLI


As the last step, we will be setting up the Sequelize-CLI package. Sequelize command line interface makes it very easy to create and modify migrations, seeders and models with just few simple commands.


Run `npm install --save-dev sequelize-cli` to install CLI package. Note that we are saving it as a dev dependency because we will not need it when the application is in production.


We will now use sequelize-cli to convert our project structure to use Sequelize features. `npx sequelize-cli init` command will automatically create some folders.


The project directory should now look as below:

├── app.js

├── bin

├── config

├── migrations

├── models

├── node_modules

├── package.json

├── package-lock.json

├── public

├── routes

├── seeders

└── views


Note that the `npx sequelize-cli init` command has added config, migrations, models and seeders directories.


### 4- Connecting to MySQL database using Sequelize


As we have already generated the project structure with Express, Sequelize and Sequelize-CLI. Let's open `config.json` file from config directory.

{
  "development": {
    "username": "root",
    "password": null,
    "database": "database_development",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}


As you can see that config.json file is very well structured, Just change the parameters in config.json file according to your DB credentials.


It is not necessary that the database should be created before adding it into the config details. However, we should always have a MySQL user with all the privileges. In most cases, you have the database and it's user available when you are going to deploy your app.


If you are working on local environment and have no issues in creating a user and assign all the privileges (including db creation privilege), You can  use following commands while in MySQL terminal as root user:


CREATE USER 'node_user'@'%' IDENTIFIED WITH mysql_native_password BY 'anypassword';
GRANT ALL PRIVILEGES ON *.* TO 'node_user'@'%';


Once you add all the details in config.json file, following sequelize-cli command can be used to create a DB in MySQL server. Skip this step if you have created the database already.


`npx sequelize-cli db:create`


 You should now be able to connect with MySQL DB using Sequelize ORM.


## 5- Creating first model and migration in Sequelize


A model is a representation of a DB table in the form of a JS object. In Sequelize ORM, we will use these objects (one for each table) to communicate with the table and performing CRUD operations.


Migrations are like version controlling of your DB, it keeps track of all the changes being made. We will create a migration whenever a table creation or alteration takes place.


Whenever we create a model using Sequelize-cli, the corresponding migration file is also created. As we have connected MySQL with node and express.js, Let's create our first migration file.


`npx sequelize-cli model:generate --name User --attributes first_name:string,last_name:string`


By running above command, we should now have a model and it's migration file created in /models and /migrations directories respectively.


/models/user.js


'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class User extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
    }
  };
  User.init({
    first_name: DataTypes.STRING,
    last_name: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'User',
  });
  return User;
};


/migrations/******-create-user.js


'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('Users', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      first_name: {
        type: Sequelize.STRING
      },
      last_name: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('Users');
  }
};


Just run `npx sequelize-cli db:migrate` and you should have a new table created named Users.


## Conclusion


In order to work with Sequelize features, We can now use Sequelize-CLI commands in our project. I will share more about migrations, relations and seeders in my upcoming posts.


If you have liked this post, Kindly consider sharing it and also subscribing to this blog.
