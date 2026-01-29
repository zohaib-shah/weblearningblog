---
layout: post
title: "Seeding data in MySQL table using Sequelize and faker.js in NodeJS"
date: 2021-07-25
categories: [JavaScript, Node.js, MySQL]
tags: [javascript, nodejs, mysql, sequelize]
---

# Seeding data in MySQL table using Sequelize and faker.js in NodeJS
In this guide, We will learn about seeding data in MySQL table using Sequelize ORM and faker.js. Seeding a table simply means adding fake data in the database for testing and demonstration purposes.


In our last [post](http://weblearningblog.com/nodejs/how-to-use-sequelize-with-node-and-express/), we did setup Sequelize with NodeJS application and created the table Users. We should start from there and add more columns to the table.


## 1- Adding more columns to the Users table


To add more columns to existing table in Sequelize, we need to create another migration using `npx sequelize-cli migration:create --name add-more-columns-users` command.


We now have new migration file created under migrations directory. Go ahead and change this file as follows:


'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.addColumn('Users','email',{ type : Sequelize.DataTypes.STRING });
    await queryInterface.addColumn('Users','ip',{ type : Sequelize.DataTypes.STRING });
    await queryInterface.addColumn('Users','phone',{ type : Sequelize.DataTypes.STRING });
    await queryInterface.addColumn('Users','address',{ type : Sequelize.DataTypes.STRING });
    await queryInterface.addColumn('Users','city',{ type : Sequelize.DataTypes.STRING });
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.removeColumn('Users','email');
    await queryInterface.removeColumn('Users','ip');
    await queryInterface.removeColumn('Users','phone');
    await queryInterface.removeColumn('Users','address');
    await queryInterface.removeColumn('Users','city');
  }
};


It should be noted here that the first argument of the removeColumn and addColumn function is the name of the table and not of Model. Now run `npx sequelize-cli db:migrate`, and you should have the Users table as below:


+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int          | NO   | PRI | NULL    | auto_increment |
| first_name | varchar(255) | YES  |     | NULL    |                |
| last_name  | varchar(255) | YES  |     | NULL    |                |
| createdAt  | datetime     | NO   |     | NULL    |                |
| updatedAt  | datetime     | NO   |     | NULL    |                |
| email      | varchar(255) | YES  |     | NULL    |                |
| ip         | varchar(255) | YES  |     | NULL    |                |
| phone      | varchar(255) | YES  |     | NULL    |                |
| address    | varchar(255) | YES  |     | NULL    |                |
| city       | varchar(255) | YES  |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+


## 2- Create your first Seeder file in Sequelize with sequeliz-cli


In order to seed the data, we will run `npx sequelize-cli seed:generate --name add-users` command. The seed file should now appear in seeders directory. Open the seed file and change it's content as follows:


'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
   await queryInterface.bulkInsert('Users',[{
    first_name: "Zohaib",
    last_name: "Shah",
    email: "abc@def.com",
    phone: "123456789",
    ip: "192.168.1.1",
    address: "Dummy Street",
    city: "Dummy City",
    createdAt : new Date(),
    updatedAt : new Date()
  }],{});
  },

  down: async (queryInterface, Sequelize) => {

     await queryInterface.bulkDelete('Users', null, {});
  }
};


Again, the first parameter of queryInterface's bulkInsert method is the name of the table in MySQL db, Don't confuse it with model name.


We will run the seed now by issuing `npx sequelize-cli db:seed:all` command. On successful execution, one record should be available in the Users table now. Following points are important to consider before moving forward with the seeders:


Unlike migrations, Sequelize doesn't keep any record of already executed seeds. Whenever we run `db:seed:all` or `db:seed --seed filename.js`, the code within the up method of the seed file will be run.
Similarly, the code in down method will run when `db:seed:undo:all` command is executed.
`db:seed:all` executes every file in seeders directory. While `db:seed --seed filename.js` runs only specific seed file.


## 3- Combining faker.js with Sequelize Seeders


If we are willing to give our application a real world look, We should have detailed and distinguished data in the DB. In my opinion, [faker.js](https://www.npmjs.com/package/faker" target="_blank" rel="noreferrer noopener nofollow) is the best available npm package to achieve this.


faker.js has some built-in objects like name, address, internet, commerce and many more to choose from for our specific requirement. These objects generate random data for different use cases.


We will utilize faker.js name, address and internet objects to fill the data. Open the seed file generated previously and modify it as follows:


```javascript
var faker = require('faker');
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    /**
     * Add seed commands here.
     *
     * Example:
     * await queryInterface.bulkInsert('People', [{
     *   name: 'John Doe',
     *   isBetaMember: false
     * }], {});
    */
   var dummyJSON = [];
   for(var i = 0 ; i  {
    /**
     * Add commands to revert seed here.
     *
     * Example:
     * await queryInterface.bulkDelete('People', null, {});
     */
     await queryInterface.bulkDelete('Users', null, {});
  }
};


In step 2, we only added one record, but here we have compiled an array of objects (JSON) to feed the bulkInsert function. Every time we call a method of faker.js object, it returns a random output. Let's run the seed again with `npx sequelize-cli db:seed:all`.


Users table now contains 5000 records as we iterates the loop 5000 times and adding an object in the array on every iteration.


| 5085 | Wyman       | Rogahn        | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Lucie_Nitzsche@hotmail.com          | 247.115.182.108 | 351.968.3165 x504     | 3647 Hartmann Route           | Hampton                     |
| 5086 | Rhianna     | Marks         | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Joey_Robel23@yahoo.com              | 214.29.12.146   | 515-497-3993 x937     | 8982 Annie Isle               | Antioch                     |
| 5087 | Gordon      | Kessler       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Stevie.Fisher66@hotmail.com         | 126.73.228.198  | (747) 798-4973        | 017 Jeramy Knolls             | San Diego                   |
| 5088 | Sigrid      | Lynch         | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Elvie16@gmail.com                   | 154.205.200.61  | (587) 353-9717 x803   | 184 Emmet Lakes               | South Bend                  |
| 5089 | Florence    | Mitchell      | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Linnea_Smith41@hotmail.com          | 2.179.113.29    | 1-694-348-3633        | 9084 Koch Stravenue           | Arcadia                     |
| 5090 | Joanne      | Feil          | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Eugenia79@gmail.com                 | 231.122.171.115 | 683-283-6576 x966     | 75873 Dayne Lodge             | Somerville                  |
| 5091 | Pasquale    | Marvin        | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Summer.Stoltenberg70@yahoo.com      | 75.156.38.31    | 237.931.2364 x819     | 5567 Keeling Shores           | Poway                       |
| 5092 | Mary        | Barrows       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Darrin.Swaniawski@hotmail.com       | 166.44.227.139  | 1-460-322-2822        | 366 Conrad Keys               | Hoboken                     |
| 5093 | Garrick     | Hilpert       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Lenny_Wehner@gmail.com              | 204.172.245.91  | 848-901-5218          | 740 Dicki View                | Perris                      |
| 5094 | Brenden     | Reinger       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Concepcion95@gmail.com              | 5.30.2.156      | (252) 970-1063        | 106 Verdie Port               | Springfield                 |
| 5095 | Alaina      | Gerlach       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Lacy.Koch14@hotmail.com             | 74.158.255.171  | 916.986.0034 x5979    | 532 Nitzsche Extensions       | Temecula                    |
| 5096 | Weldon      | Russel        | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Roberto14@yahoo.com                 | 115.240.124.154 | (631) 827-0600        | 6717 McKenzie Rest            | Union City                  |
| 5097 | Edd         | Roob          | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Samanta12@gmail.com                 | 196.72.109.198  | 876-877-8820 x70846   | 0513 Van Stravenue            | Tampa                       |
| 5098 | Lea         | Labadie       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Alverta.Tromp33@yahoo.com           | 55.110.39.213   | (414) 553-9077        | 7282 Brandy Oval              | Canton                      |
| 5099 | Cecilia     | Prosacco      | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Quincy96@hotmail.com                | 143.113.0.129   | 1-431-250-4038        | 3480 Stamm Junction           | Virginia Beach              |
| 5100 | Arthur      | Abshire       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Jake.Hodkiewicz@yahoo.com           | 112.70.233.80   | (634) 852-3985 x8952  | 8234 Ernesto Lodge            | Escondido                   |
| 5101 | Marjolaine  | Goodwin       | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Thurman.Nitzsche75@hotmail.com      | 95.151.250.100  | 761-524-8051          | 5921 Bode Springs             | Kokomo                      |
| 5102 | Keegan      | Haley         | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Milo8@gmail.com                     | 80.235.71.131   | 660-391-8135 x92773   | 09435 Cronin Bridge           | Wheaton                     |
| 5103 | Chauncey    | Lindgren      | 2021-07-17 16:37:08 | 2021-07-17 16:37:08 | Kaitlyn_Walter42@hotmail.com        | 27.34.232.40    | (645) 703-8657        | 8328 MacGyver Gateway         | Lake Havasu City            |
+------+-------------+---------------+---------------------+---------------------+-------------------------------------+-----------------+-----------------------+-------------------------------+-----------------------------+
5050 rows in set (0.17 sec)


I already have some records added before this is why the 5050 rows are being returned.


## Conclusion:


No matter how good backend logic your application has, when you present it to the client it should give a good first impression. The UI should look decent which is only possible when content is available in the DB.


If you have liked this post, consider subscribing to my blog and sharing it.

```