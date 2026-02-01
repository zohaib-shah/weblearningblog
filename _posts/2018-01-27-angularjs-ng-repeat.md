---
layout: post
title: "AngularJS ng-repeat"
date: 2018-01-27
categories: [AngularJS]
tags: [javascript, angular]
---

AngularJS ng-repeat is a useful concept in any of the AngularJS based app. AngularJS ng-repeat directive is used to iterate through a JSON and build HTML accordingly. Filters in AngularJS can be useful if we want to filter the rendered HTML as per our needs.


The exciting factor while working with AngularJS HTML rendering is that it is seamless and we don't need to go to the server to fetch the updated HTML as everything is happening on the client side.

 If you have been working with any server side language and experienced a situation where you need to develop a product page and give visitor the option pane where he could narrow the product list by selecting preferences like size,color,style etc, You would have built a system where ajax requests are fetching the record from server every time visitor changes his preference.


What happens in AngularJS ng-repeat with AngularJS filters is very different from above mentioned classical approach. In AngularJS, We would just have fetched a complete products JSON from server and then this JSON would be queried every time visitor changes his preference. This way, most of the processing is done on the client side.


Let's explore some practical code examples to see what we have discussed so far. Following are the steps which will demonstrate how AngularJS ng-repeat works:


Structure of files for AngularJS APP
Few words on AngularJS directives used here
Creating AngularJS ng-repeat table with JSON
Understanding the AngularJS Code for AngularJS ng-repeat


## Structure of Files for AngularJS


Let's see how we setup our files to have a working AngularJS enabled HTML page which could demonstrate AngularJS ng-repeat with AngularJS filters.


AngularJS ng-repeat


You can download AngularJS from [here.](https://angularjs.org/" target="_blank" rel="noopener noreferrer) Make sure you download AngularJS as Angular 2 is a total revamp of AngularJS and ultimately not compatible with AngularJS. Once downloaded just include it in your HTML file and include one more JS file where we could write our AngularJS code, in this case we have named it as app.js


## Few words on AngularJS directives used here


AngularJS directives are used to modify default HTML elements.
 AngularJS directives start with ng-, There are so many native directives available with AngularJS and we can also create custom directives, Following are some useful directives with a little description:


ng-app: ng-app is a kind of container directive, it contains all the elements of our app .i.e. HTML page. Anything inside a parent DOM element with ng-app directive, could be control with our Angular Code.
ng-controller: An AngularJS controller is basically a logical division of HTML DOM. Going further with this post, we will see this more clearly.
ng-repeat: The ng-repeat directive tells an HTML element to repeat itself with each iteration of a JSON.
ng-bind: The ng-bind directive let's us bind a variable in $scope with HTML element.


Remember that if we have to use custom attributes with any HTML element, we have to prefix it with data- in order to make it pass the HTML validation rules. Same is true for AngularJS ng- based attributes, we will be prefixing all these attributes with data-. Therefore, ng-repeat will become data-ng-repeat and so on.


## Creating AngularJS ng-repeat table


Shown below is the HTML file content:


AngularJS ng-repeat


		NameCategoryPriceRatingStore


This is just a normal HTML code, Apart from some AngularJS directives. The first directive is data-ng-app or just ng-app, every HTML inside the div with ng-app can be modified by the AngularJS code. Next directive is AngularJS ng-controller, ng-controller is a logical section of our app, since it is basic example so we will only have one section, this is why we assign ng-controller directive to the same div where ng-app is assigned.


The most important directive is ng-repeat on line # 11, Here, we have assigned ng-repeat to a table row element "tr". In simple english we are asking table row to repeat itself for every iteration of "products" JSON in $scope of current controller. Here "products" is a JSON array of objects with each object containing information related to a product. On each iteration, new product object is copied to product variable, Notice the expression inside ng-repeat .i.e. "product in products".

Now, with each row having one product, we are binding table data element "td" to relevant property of product object. In expression "product in products", The left hand side's "product" could be anything we want to name it, but on the right hand side, "products" should be existing in our AngularJS code as a $scope variable.


## Understanding AngularJS code for AngularJS ng-repeat


We have created the HTML, Now it's time to write the AngularJS code which could support our HTML. Have a look at the code snippet below:


```javascript
var app = angular.module('my-app',[]);
app.controller('my-controller',function($scope){
$scope.products = [
{name : "Formal Shirt",category: "Clothing" , price: 50 , rating : 7 , store : "ABC Store"},
{name : "Jeans",category: "Clothing" , price: 70 , rating : 7 , store : "Dummy Store"},
{name : "LED TV",category: "Electronics" , price: 500 , rating : 9 , store : "Electronics Store"},
{name : "Fitness Machine",category: "Health & Sports" , price: 150 , rating : 6 , store : "Health Store"},
{name : "Juicer Machine",category: "Cooking" , price: 250 , rating : 4 , store : "Cooking Shop"},

];
});


On line # 1, we are defining an AngularJS module. The AngularJS module contains filters, controllers, services and etc. So, we are creating a module name "app". Notice the parameter "my-app", this is same string as we defined in ng-app directive in our HTML. At line # 2, we are informing the "app" module to create a controller with the name "my-controller" as defined in ng-controller directive of HTML. $scope is a big topic, for now just take it as an object which will hold all the variables and functions accessible by the current controller.

 The last thing is products JSON array of objects which is self explanatory, We are creating a property on $scope object and naming it as products, each property or variable of $scope is accessible in AngularJS code of current controller and in HTML DOM which is dedicated to this controller, in our case, it is the div with ng-controller attribute "my-controller".


## Conclusion


When I first started learning AngularJS, it was looking a very difficult technology and my traditional learning approach was not working in that case. I changed my approach and adapted a different strategy, which was to code and see. This way I was able to understand it.


Although, this post has already become quite lengthy, but still we are not done. In my upcoming tutorials, I will show you how to add filters to AngularJS ng-repeat. I will also write about how we can fetch JSON array from server instead of the static JSON as in this post.

```