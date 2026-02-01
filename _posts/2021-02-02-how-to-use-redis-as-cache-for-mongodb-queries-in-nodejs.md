---
layout: post
title: "How to use Redis as Cache for MongoDB queries in NodeJS"
date: 2021-02-02
categories: [NodeJS]
tags: [javascript, nodejs, database, redis, mongodb]
---

Using Redis as a cache for MongoDB queries in NodeJS can make your application super fast. The reason for faster access is due to the fact that Redis stores data in memory while MongoDB stores data in disk.


## Where Caching is useful ?


Caching is useful in all scenarios where we need to access the same data frequently.


In an E-Commerce store, we need to run a campaign against one particular product. It would be good if we pick the product from traditional DB and save it in Redis Cache.


In a REST API, we have different configurations for different users. On each API hit, fetching  users preferences / settings from MySQL or MongoDB will slow down the response time. From my personal experience, it helped me greatly when I had to place this logic in one of the flight search websites to optimize the response time.


## Setting up a NodeJS app with Redis and Mongo


Install MongoDB database if not already installed `sudo apt install mongodb`.Install Redis Server `sudo apt install redis-server`.Create a directory to contain the project `mkdir node-redis-mongo`. cd into the project directory.Run `npx express-generator` to create an application file structure.Type `npm install` to install all the dependencies.`npm install mongoose` to install mongoose package which will communicate with MongoDB.`npm install redis` to install redis package which will be used to interact with Redis Server.


Alternately, you can clone the repo from `git clone https://github.com/zohaib-shah/node-redis-mongo.git`


## What we are building ?


We will create a simple student form and it's related collection in MongoDB. Whenever a student is saved, it is saved in both MongoDB and Redis.


To view specific student, we first try from Redis and then from MongoDB.


We will save all our configuration in `config.js` file. Save `config.js` file in root of the application. We will access parameters from this file whenever needed.


{
	"mongo_host":"127.0.0.1",
	"mongo_db":"students",
	"mongo_port":"27017",
	"redis_host":"127.0.0.1",
	"redis_port":"6379"

}


## Creating Schema for MongoDB collection


Mongoose asks us to have a schema for each MongoDB collection. Create a new directory in project root and name it "models". Inside "models", create a file `Student.js` and add following code:

```javascript
var mongoose = require("mongoose");

var StudentSchema = new mongoose.Schema({
   first_name : String,
   last_name : String,
   class : String,
   age : Number
},{timestamps: true});

var Student = mongoose.model('Student', StudentSchema);

module.exports = Student;


## Creating Views


We will only have two views .i.e. `student.jade` and `edit_student.jade`.

student.jade
extends layout
block content
	h1 Student Form
	p
		form(method='POST' action='/post-student')
			p
				span First Name
				input(type='text' name='first_name')
			p
				span Last Name
				input(type='text' name='last_name')
			p
				span Class
				input(type='text' name='class')
			p
				span age
				input(type='number' name='age')
			p
				input(type='submit' value='Add Student')
	p
		table(border=1)
			tr
				th ID
				th First Name
				th Last Name
				th Class
				th Age
				th Edit
			each st in students
				tr
					td #{st.id}
					td #{st.first_name}
					td #{st.last_name}
					td #{st.class}
					td #{st.age}
					td
						a(href="/edit-student/"+st.id) Edit


edit_student.jade
extends layout
block content
	h1 Edit Student Form (Picked from #{source})
	p
		form(method='POST' action='/post-student')
			input(type='hidden' name='student_id' value=st.id)
			p
				span First Name
				input(type='text' name='first_name' value=st.first_name)
			p
				span Last Name
				input(type='text' name='last_name' value=st.last_name)
			p
				span Class
				input(type='text' name='class' value=st.class)
			p
				span age
				input(type='number' name='age' value=st.age)
			p
				input(type='submit' value='Edit Student')


Add both files in `views` directory.


## Saving the record in MongoDB and Redis


The starting point of our application is `app.js` file in project root. Following snippet from `app.js` suggests that it is including all the URLs from `routes/index.js` file as root URLs.

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
app.use('/', indexRouter);
app.use('/users', usersRouter);

Load the student form:

Open the `index.js` file from `routes` directory and add below code:

var express = require('express');
var redis = require('redis');
var router = express.Router();
var Student = require('../models/Student');
var mongoose = require('mongoose');
var config = require('../config.json');
router.get('/add-student', (req, res, next) => {
mongoose.connect("mongodb://"+config.mongo_host+":"+config.mongo_port+"/"+config.mongo_db, {useNewUrlParser: true, useUnifiedTopology: true}, async(err) => {
  var students = await Student.find({}).exec();
  res.render('student', { title: 'Add Student', students : students });
});

});


Now, open the URL http://127.0.0.1:3000/add-student, it should display a basic HTML form as below:


   Please ignore the styling and validation of this form for now.


As the action of the the "student" form is set to `action='/post-student'`, let's create a POST function in `routes/index.js` file with following code:

router.post('/post-student', function(req, res, next) {
  console.log(req.body);
  mongoose.connect("mongodb://"+config.mongo_host+":"+config.mongo_port+"/"+config.mongo_db, {useNewUrlParser: true, useUnifiedTopology: true}, async(err) => {
  if(!err){
  var st = new Student({
  	first_name : req.body.first_name,
  	last_name : req.body.last_name,
  	class : req.body.class,
  	age : req.body.age
  });
  st.save(() => {
  	console.log("Student has been saved");
  	var client = redis.createClient({host:config.redis_host,port:config.redis_port});
	client.set(['st-'+st.id,JSON.stringify(st.toJSON())],(err,reply)=>{
		if (err) throw err;
		console.log(reply);
	});
  	res.redirect('/add-student');
  });
	} else {
		console.log("Can  not connect with DB");
	}
});

});


First of all, we are connecting with MongoDB. If the connection is successful, we are then creating a mongoose object by instantiating the `Student` schema.


Here, the `Student` data is being picked directly from Form body which is not a good practice. We should ideally place validation logic before this step.


Finally, we are calling the `save()` method of mongoose model. Callback for this `save()` method is also being used. Check more on mongoose [here](https://mongoosejs.com/docs/" target="_blank" rel="noopener noreferrer).


We have now saved the data in MongoDB collection using Mongoose. It is now time for storing the same data in Redis database.


To cache MongoDB data in Redis, different strategies may be used. For simplicity, we will only save the data in Redis when it is being saved in the MongoDB.


However, the more practical approach could be to watch for data on periodic basis and check if it is already available in Redis cache. If not, add the required data in redis cache. Similarly, check for the outdated data in Redis cache and remove it from cache if it is no more required.


### Using the mongoose save() callback to cache data in Redis server


The `save()` method callback is defined as an arrow function. This callback function will execute when mongoose has finished storing the Student record in MongoDB.


()=>{
  	console.log("Student has been saved");
  	var client = redis.createClient({host:config.redis_host,port:config.redis_port});
	client.set(['st-'+st.id,JSON.stringify(st.toJSON())],(err,reply)=>{
		if (err) throw err;
		console.log(reply);
	});
  	res.redirect('/add-student');
  }


Usually, it is a good practice to generate unique redis keys by prefixing database IDs with some sort of meaningful grouping text.


Let's understand the redis code line by line:


`var client = redis.createClient({host:config.redis_host,port:config.redis_port});`: Creates a Redis client.
`client.set(['st-'+st.id,JSON.stringify(st.toJSON())]`: Converts mongoose model into JSON friendly format and then save against a unique redis key having format like 'st-mongodocumentid'


### View data in Redis database using redis-cli tool


To check the saved data in Redis, we have a command line tool from Redis called [redis-cli](https://redis.io/topics/rediscli" target="_blank" rel="noopener noreferrer). To view 10 recent keys, type `redis-cli --scan | head -10`.


## View MongoDB document from Redis Cache


As we have saved a record in MongoDB and in Redis cache as well. We will now utilize the redis cache to read the record. For this purpose, GET /edit-student/:std_id route should be created. Through this URL,  we will fetch the data from redis based on the provided std_id (student id). If the data is unavailable, it will then try to retrieve the data from MongoDB.


Inside `routes/index.js` file, add the following code:

router.get('/edit-student/:std_id', (req, res, next)=>{
//first try to fetch from redis
var client = redis.createClient({host:config.redis_host,port:config.redis_port});
client.get("st-"+req.params.std_id, function(rd_err, rd_result) {
    if (rd_err){//if error occured or key doesnt exist
    	//fetch from DB
    	mongoose.connect("mongodb://"+config.mongo_host+":"+config.mongo_port+"/"+config.mongo_db, {useNewUrlParser: true, useUnifiedTopology: true}, async(err) => {
  var st = await Student.findById(req.params.std_id).exec();
  res.render('edit_student', { title: 'Edit Student', st : st , source : 'Mongo DB' });
});
    } else {
  			//console.log(rd_result);
  			var st = Student.hydrate(JSON.parse(rd_result));
  			console.log(st);
  			res.render('edit_student', { title: 'Edit Student', st : st , source : 'Redis Cache' });
    }


});
});


Finally, as long as the data is available in redis, it will be picked from redis cache. Hence, the response time of the page will be ideal.


## Conclusion:


We have seen basic method of implementing redis as cache for MongoDB queries. Though, the code is simple but it is also adaptable for bigger concepts and use cases.


If you have liked this post, Kindly consider sharing and subscribing this blog.

```