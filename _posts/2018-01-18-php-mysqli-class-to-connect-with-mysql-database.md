---
layout: post
title: "PHP mysqli Class to Connect with MySQL Database"
date: 2018-01-18
categories: [PHP]
tags: [php, mysqli, mysql, database, web-development]
---

PHP mysqli class is one of the multiple ways to connect with MySQL database using PHP. In this post we will learn about PHP mysqli class, its useful methods and properties.

Following logical steps will show you how to use mysqli class to access MySQL database in PHP:

- Connecting with MySQL database using PHP
- Creating a Table in MySQL Database using PHP mysqli
- Inserting data in MySQL database using mysqli class
- Update data using mysqli class in PHP
- Fetch Associated array from MySQL table
- Fetch table row as object using mysqli class

## Connecting with MySQL Database using PHP mysqli

To establish connection, we need to create an object of PHP mysqli class. Once the object is created, we will check its `connect_errno` property. If connection is successful, `connect_errno` property will hold 0 otherwise it will hold the MySQL error code as described [here](https://dev.mysql.com/doc/mysql-errors/8.0/en/server-error-reference.html).

```php
<?php
$mysqli = new mysqli('127.0.0.1','root','','weblearningblog');
/*connecting with mysql*/
if(!$mysqli->connect_errno){
    echo "Connection Successful";
} else {
    echo $mysqli->connect_error;
}
/*connecting end*/
?>
```

Notice the order of the parameters of mysqli constructor: host, username, password and database name respectively.

## Creating a Table in MySQL Database using PHP mysqli

To create a new table in database, we use `query()` method of mysqli class. This method returns FALSE if query fails. In queries where a result set is expected, `query()` method will return result set and for other successful queries `query()` method returns TRUE.

```php
<?php
$mysqli = new mysqli('127.0.0.1','root','','weblearningblog');
/*creating a table starts*/
if($mysqli->connect_errno){
    echo $mysqli->connect_error;
} else {
    $sql = 'CREATE TABLE students(
        id INT NOT NULL AUTO_INCREMENT,
        name varchar(30) NOT NULL,
        study_program varchar(20) NOT NULL,
        department varchar(20),
        date_added TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
        PRIMARY KEY (id)
    )';
    $result = $mysqli->query($sql);
    if(!$result){
        echo "Query Failed. ".$mysqli->error;
    } else {
        echo "Query Successful";
    }
    $mysqli->close();//close the connection
}
/*creating a table ends*/
?>
```

CREATE TABLE is SQL command therefore, discussing it is not in the scope of this article. We store SQL query in a string, later this string is passed as first and only required parameter for mysqli `query()` method.

## Inserting Data in MySQL Database using mysqli Class

Inserting records in MySQL database using mysqli class is similar to the previous section as the process is same but we will use a different SQL Command INSERT INTO for this purpose.

```php
<?php
$mysqli = new mysqli('127.0.0.1','root','','weblearningblog');
if($mysqli->connect_errno){
    echo $mysqli->connect_error;
} else {
    $sql = 'INSERT INTO students (name,study_program,department) VALUES("John","BS","Computer Science")';
    $result = $mysqli->query($sql);
    if(!$result){
        echo "Query Failed. ".$mysqli->error;
    } else {
        echo "Successfully Inserted. ".$mysqli->affected_rows." Records";
    }
}
?>
```

The mysqli class `affected_rows` property is useful in insert and update operations as it holds the number of rows which query has affected. In this case, it will hold 1 as this query is inserting one row in table.

For inserting multiple records at once, we just need to change the SQL command INSERT INTO as follows:

```php
<?php
$mysqli = new mysqli('127.0.0.1','root','','weblearningblog');
if($mysqli->connect_errno){
    echo $mysqli->connect_errno;
} else {
    $sql = 'INSERT INTO students
    (name,study_program,department) VALUES
    ("John","BS","Computer Science"),
    ("Kane","MS","Computer Science"),
    ("Bob","PhD.","Physics"),
    ("Stewart","MBA","Computer Science")';
    $result = $mysqli->query($sql);
    if(!$result){
        echo "Query Failed. ".$mysqli->error;
    } else {
        echo "Successfully Inserted. ".$mysqli->affected_rows." Records";
    }
}
?>
```

## Update Data using mysqli Class in PHP

Updating the record in MySQL table using mysqli class is pretty straightforward.

```php
<?php
$mysqli = new mysqli('127.0.0.1','root','','weblearningblog');
if($mysqli->connect_errno){
    echo $mysqli->connect_errno;
} else {
    $sql = 'UPDATE students SET name="Kane" WHERE id=1';
    $result = $mysqli->query($sql);
    if(!$result){
        echo "Query Failed. ".$mysqli->error;
    } else {
        echo "Successfully Updated. ".$mysqli->affected_rows." Records";
    }
}
?>
```

## Fetch Associated Array from MySQL Table using mysqli

Reading data from MySQL table using PHP mysqli extension involves retrieving a result set from table and then iterate through it by fetching record as associative array one by one.

```php
<?php
$mysqli = new mysqli('127.0.0.1','root','','weblearningblog');
if($mysqli->connect_errno){
    echo $mysqli->connect_errno;
} else {
    $sql = 'SELECT * FROM students';
    $result = $mysqli->query($sql);
    if(!$result){
        echo "Query Failed. ".$mysqli->error;
    } else {
        if($result->num_rows > 0){
            $html = "<table>";
            $html .= "<tr><th>ID</th><th>Name</th><th>Study Program</th><th>Department</th></tr>";
            while($current_row = $result->fetch_assoc()){
                $html .= '<tr>';
                $html .= '<td>'.$current_row['id'].'</td>';
                $html .= '<td>'.$current_row['name'].'</td>';
                $html .= '<td>'.$current_row['study_program'].'</td>';
                $html .= '<td>'.$current_row['department'].'</td>';
                $html .= '</tr>';
            }
            $html .= "</table>";
            $result->free();//free the resultset memory
            echo $html;
        } else {
            echo "No Record Exists";
        }
    }
}
?>
```

As shown in code snippet above, the `query()` method is now returning a result set instead of TRUE or FALSE as in insert and update operations discussed in previous sections. Here we are getting the students records from MySQL table and creating an HTML table to view it in a browser.

`num_rows` property of result set object is having the number of rows or records this result set is consisting of, therefore it is a good idea to evaluate `num_rows` property before using the result set to read data.

While reading data from database table, it is very important to learn how `fetch_assoc()` method of result set object works. Whenever we call `fetch_assoc()` method on result set object, it gives us one row and moves internal data pointer to next row, which means if we call `fetch_assoc()` again, it will return next row and so on. Ultimately if we use `fetch_assoc()` in a while loop, it will execute on each iteration and returns a new row. If there is no record left `fetch_assoc()` will return NULL which will cause while loop to exit.

As `fetch_assoc()` returns associative array, the key of the array is column name of the table and value of the array is the column value for current row. Building up of HTML table is straightforward and doesn't need any explanation.

## Fetch Table Row as Object using mysqli Class

To fetch record as object instead of associative array, we just have to replace `fetch_assoc()` method with `fetch_object()`.

```php
<?php
while($current_row = $result->fetch_object()){
    $html .= '<tr>';
    $html .= '<td>'.$current_row->id.'</td>';
    $html .= '<td>'.$current_row->name.'</td>';
    $html .= '<td>'.$current_row->study_program.'</td>';
    $html .= '<td>'.$current_row->department.'</td>';
    $html .= '</tr>';
}
?>
```

## Conclusion

In this tutorial, I tried to focus on practical and code-oriented approach to teach this topic. Please share your valuable feedback by commenting.
