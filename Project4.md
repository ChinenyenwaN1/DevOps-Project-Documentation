# MEAN-STACK

## What is Mean stack?
**Mean stack is a user-friendly full stack javascript framework which is highly preferable for creating dynamic websites and apps. It is free and open source stack developed to offer developers a fast and highly organized method for building a rapid prototype of mean stack development based web apps.
The Mean stack is ideal for building all kinds of apps from dynamic single page apps to complex enterprise and social media apps. Mean stack is proposed to deliver a straightforward and initial point for cloud-native full-stack web apps. Mean stack development is a set of open source components that collectively offer a comprehensive framework for creating dynamic web apps.**

**MEAN stands for:** 
* MongoDB 
* Express.js 
* AngularJS 
* Node.js.

Mean stack is ideal for building all kinds of apps because it works on only one language in the whole project, its efficiency and flexibility, an ideal choice for businesses, High reusability and speed, Cloud comparability, easy to learn and budget friendly for users.

## MEAN STACK is comprised of four different technologies:
* **M**ongoDB (Document database): Stores and retrieve data.
* **E**xpress JS (Back-end application framework): Makes requests to Database and return a response
* **A**ngularJS (Front-end application framework): Handles Client and Server Requests
* **N**ode.js (JavaScript runtime environment): Accept requests and display results to end user

![image](https://user-images.githubusercontent.com/116161693/206679405-25d79aca-cec2-41ea-b700-c7b4146983a3.png)

**There are variations to the MEAN stack such as MERN (replacing Angular.js with React.js) and MEVN (using Vue.js). The MEAN stack is one of the most popular technology concepts for building web applications.**

# How Does the MEAN Stack Work?

## MEAN Stack Architecture
The MEAN architecture is designed to make building web applications in JavaScript and handling JSON incredibly easy.

![image](https://user-images.githubusercontent.com/116161693/206679486-0ea2bde5-cde7-46e0-86de-ebc312cdc8bb.png)

# MEAN Stack Components

## MongoDB

**MongoDB** is an open source, NoSQL database designed for cloud applications. It uses object-oriented organization instead of a relational model. It stores data in the key-value pair, using binary data type like JSON. MongoDB is an ideal choice for a database system where we need to manage large sized tables with millions of data. Moreover, including a field to MongoDB is easier as it does not require updating the entire table. With MongoDB we can develop an entire application with just one application which is JavaScript.


## Express

**Express** is a mature, flexible, lightweight server framework. It is designed for building single, multi-page, and hybrid web applications. This lightweight framework uses the Pug engine to provide support for templates. Express is a web application framework for Node.js. It handles all the interactions between the frontend and the database, ensuring a smooth transfer of data to the end user.


## Angular

**Angular** JS is an open-source JavaScript framework and it is maintained by Google. The goal of this framework is to introduce MVC(Model View Controller) architecture in the browser-based application that makes the development and testing process easier. The framework helps us create a smarter web app that supports personalization.
AngularJS allows us to use HTML as a template language. Therefore, we can extend HTML's syntax to express the components of our application. Angular features like dependency injection and data binding eliminate plenty of code that we need to write.


## Node.js

**Node.js** is an open source JavaScript framework that uses asynchronous events to process multiple connections simultaneously. It allows developers to create web servers and build web applications on it. It's a server-side Javascript execution environment and also an ideal framework for a cloud-based application, as it can effortlessly scale requests on demand.
Node.js uses a non-blocking and event-driven I/O model. This makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices. We’re likely to find Node.js behind most well-known web presences.

![image](https://user-images.githubusercontent.com/116161693/206679564-5c166e99-9808-490b-8ba9-34fa35489124.png)

# MEAN STACK USE CASES

**While the MEAN stack isn’t perfect for every application, there are many uses where it excels. It’s a strong choice for developing cloud native applications because of its scalability and its ability to manage concurrent users. The AngularJS frontend framework also makes it ideal for developing single-page applications that serve all information and functionality on a single page. Here are a few examples for using MEAN:**

* Calendars
* Expense tracking
* News aggregation sites
* Mapping and location finding

![image](https://user-images.githubusercontent.com/116161693/206679627-a9ab213e-2aff-4478-818b-1f8179bc4f05.png)

# In this Project we are going to deploy a web based application on our Linux server using MEAN STACK with the following steps.

# Step 0 – Preparing prerequisites

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account – go back to Project 1 Step 0 to sign in to AWS free tier account ans create a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image.


# Step1: Install NodeJs

## Task
In this assignment you are going to implement a simple Book Register web form using MEAN stack.

## We will install nodejs on our server

Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

**Update ubuntu**

```
sudo apt update
```

**Upgrade ubuntu**

```
sudo apt upgrade
```

**Install NodeJS**

```
sudo apt install -y nodejs
```

# Step 2: Install MongoDB

**MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.**

* **Run these commands:**

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

**OUTPUT:


```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

**OUTPUT:

## Install MongoDB

```
sudo apt install -y mongodb
```

**Start The server**

```
sudo service mongodb start
```

**Verify that the service is up and running**

```
sudo systemctl status mongodb
```

**OUTPUT:

Install [npm](https://www.npmjs.com) – Node package manager.

```
sudo apt install -y npm
```

## Install ‘body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

```
sudo npm install body-parser
```

**OUTPUT:**


**Create a folder named ‘Books’**

```
mkdir Books && cd Books
```

**In the Books directory, Initialize npm project**

```
npm init
```

**OUTOUT:**

**Add a file to it named server.js**

```
touch server.js
```

```
vi server.js
```

**Copy and paste the web server code below into the server.js file.**

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```

**OUTPUT:**


# Step 3: Install Express and set up routes to the server

**Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.**

**We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.**

```
sudo npm install express mongoose
```

**OUTPUT:**

**In ‘Books’ folder, create a folder named apps**

```
mkdir apps && cd apps
```

**Create a file named routes.js**


```
touch routes.js
```

```
vi routes.js
```

**Copy and paste the code below into routes.js**


```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```


**OUTPUT:**


**In the ‘apps’ folder, create a folder named models**

```
mkdir models && cd models
```

**Create a file named book.js**

```
touch book.js
```

```
vi book.js
```

**OUTPUT:**


**Copy and paste the code below into ‘book.js’**


```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```


# Step 4: Access the routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.


**Change the directory back to ‘Books’**

```
cd ../..
```

**Create a folder named public**

```
mkdir public && cd public
```

**Add a file named script.js**

```
touch script.js
```

```
vi script.js
```


**Copy and paste the Code below (controller configuration defined) into the script.js file.**


```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```

**OUTPUT:**

**In public folder, create a file named index.html;**

```
touch index.html
```

```
vi index.html
```

**Copy and paste the code below into index.html file.**

```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```

**OUTPUT:**


**Change the directory back up to Books**

```
cd ..
```

**Start the server by running this command:**

```
node server.js
```

**OUTPUT:**


## The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

```
curl -s http://localhost:3300
```


It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

For this – you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.

You are supposed to know how to do it, if you have forgotten – refer to Project 1 (Step 1 — Installing Apache and Updating the Firewall)

Your Security group shall look like this:


**OUTPUT:**

Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

Quick reminder how to get your server’s Public IP and public DNS name:

You can find it in your AWS web console in EC2 details

**Run curl -s http://169.254.169.254/latest/meta-data/public-ipv4 for Public IP address or curl -s http://169.254.169.254/latest/meta-data/public-hostname for Public DNS name.**

This is how your Web Book Register Application will look like in browser:


