---
layout: post
title: "Perform CRUD Operations using Laravel Eloquent"
date: 2018-12-21
categories: [Laravel]
tags: [laravel]
---

In this post we will learn about how to perform CRUD Operations using Laravel Eloquent. Eloquent is the laravel way to talk to the database.

In upcoming posts, we will learn Eloquent in more depth, But this post will only focus on single table operations.

We will cover the post in following steps:

 	Some points on Eloquent
 	Creating MySQL table to be used with eloquent using migration
 	Creating Eloquent Model for MySQL table
 	Performing Insert using Eloquent
 	Retrieve data using Eloquent
 	Update MySQL table using Eloquent
 	Delete record using Eloquent


## Some points on Eloquent

Before moving forward, we should review some important points about laravel Eloquent:

 	For each table in your MySQL table, You should have a dedicated model.
 	A laravel eloquent model is actually a PHP class which can live anywhere but it should always extend `Illuminate\Database\Eloquent\Model`.
 	Name of your eloquent model should be singular while the table name for this model should be equal to the plural name of the model .i.e. if your model name is Article, it would always look for a table named articles in your database unless otherwise defined in your model class by defining $table property.
 	By default, an eloquent model assumes that the primary key of the table is named as id. If this is not the case, you have to specify a protected property in your eloquent model $primaryKey and assign it the name of the primary key column of your table.
 	Eloquent model also expects that the table have timestamp columns created_at and updated_at. This time, we have options to either turn it off or tell our model to use different names for these columns by specifying `const CREATED_AT = "your_created_at_column";` and `const UPDATED_AT = "your_updated_at_column"`


## Creating MySQL table to be used with eloquent using migration

We will create a table articles using migration. Go ahead and type following command:

`php artisan make:migration article --table articles --create articles`

A new migration file has been created in migrations directory with following code:

```php
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class Articles extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->increments('id');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('articles');
    }
}
```

You can learn more about migrations [here](http://weblearningblog.com/laravel/use-laravel-database-migrations/).

As you can see, only id and timestamps fields are included in this migration file. We will add more columns by editing this file as below:

```php
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class Articles extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->text('content');
            $table->integer('rating');
            $table->string('author');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('articles');
    }
}
```

Now run `php artisan migrate` to create the table in the database.

## Creating Eloquent Model for MySQL table


We have created the "articles" table in the database using migration. The next step is creating related model for this table. We need to run following artisan command to create eloquent model for "articles" table:

`php artisan make:model Article`

We can now see that Article.php file has been created inside app directory.


The name of  the model is "Article" and the table which it expects is plural of model name .i.e. "articles". Moreover, our table has an incrementing field "id" and timestamp columns "created_at" and "updated_at". Which means we are already following all the conventions set by laravel and therefore don't need to define anything explicitly in the model.


## Performing Insert using Eloquent


Practically, we would have an HTML form to take information regarding the article. This form should than be submitted to a defined route which is being served by a controller function, but to remain focused on the topic, we are skipping these steps.


Remember, you need to add the `use App\Article;` at the beginning of your controller.


Inserting records using eloquent is as simple as creating an object of eloquent model, populate its properties which are identical to the name of the table columns. Finally, calling the `save()` method of this object.

```php
$article = new Article;
    	$article->title = 'My First Article';
    	$article->content = 'Hello, This is my first post.';
    	$article->rating = 7;
    	$article->author = 'Zohaib';
    	$article->save();
```

While the above method looks very simple, we have another simpler way to insert data in the table using one line of code which is called "Mass assignment". Before we discuss mass assignment, it is important to know about fillable and guarded properties of eloquent models.


### The $fillable and $guarded properties:


While using mass assignment in insertion or updation. It is mandatory to define either $fillable or $guarded property in eloquent model. $fillable whitelists columns to be used in mass assignment while the $guarded restrict the columns to be used in mass assignment operations.


Let's insert record using mass assignment method:


$article = Article::create([
'title'=>'My Second Article',
'content'=>'Hello',
'rating'=>9,
'author'=>'Zohaib'
]);


This method is simpler because we only need to provide an associative array to the `create()` method of eloquent model class. The keys of the associative array holds the column names of the table and the values are the intended values for the row. It is important to note here, all the keys of this associative arrays should be in $fillable property or these keys should not be in $guarded property.


It should be noted here that if we are restricting any column using $guarded property, this column should have a default value.


Following is the Article model with $fillable property defined:


Mass assignment is useful if we need to create records without mentioning each property one by one. Practically, we also don't provide associative array like we did above. If an HTML form is posted to a laravel controller function. All the values are available using `request()->all()` method. Suppose if all the form fields are named as table column names, we can just get all the data with `request()->all()` and provide it to `create()` method like `Article::create(request()->all())`


## Retrieve data using Eloquent


Retrieving data is very easy, we can use `find()` method of eloquent model. `find()` method expects id of the record as integer or as array of many IDs.


$article = Article::find(1);
echo $article->title;
$articles = Article::find([1,2,3]);
foreach($articles as $a){
echo $a->title;
}


We can also use query builder methods with our eloquent models as follows:


$articles = Article::where('rating','=',7)->get();
foreach($articles as $a){
echo $a->title;
}


Retrieving all rows from table is also simple, we will call `all()` method of eloquent model.


$articles = Article::all();
foreach($articles as $a){
echo $a->title;
}


## Update MySQL table using Eloquent


After getting the record from database, we may update them simply by changing the property value of the retrieved object and then call the `save()` method again.


$article = Article::find(1);
$article->content = "My Updated title";
$article->save();


Another way of updating records is using update method which expects an associative array.


Article::where('rating','=',7)->update(['rating'=>5]);


## Delete record using Eloquent


Deleting record from database in eloquent is done by calling `delete()` method of eloquent model.


$article = Article::find(1);
$article->delete();


Alternately, we can directly delete records using primary key as below:


Article::destroy(1);


## Conclusion


You can see that how easy it is to perform CRUD operations using laravel eloquent. This post is just a summary of eloquent there are many other available methods which are useful.


I hope you like this post. Let me know with your valuable thoughts.
