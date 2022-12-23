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

![image](https://user-images.githubusercontent.com/116161693/209232557-2f66f088-8ed6-4c23-867f-0d066d43792f.png)

**There are variations to the MEAN stack such as MERN (replacing Angular.js with React.js) and MEVN (using Vue.js). The MEAN stack is one of the most popular technology concepts for building web applications.**

# How Does the MEAN Stack Work?

## MEAN Stack Architecture
The MEAN architecture is designed to make building web applications in JavaScript and handling JSON incredibly easy.

![image](https://user-images.githubusercontent.com/116161693/209230013-e4a42bac-1617-4ee4-a7af-44d219853e01.png)

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

# MEAN STACK USE CASES

**While the MEAN stack isn’t perfect for every application, there are many uses where it excels. It’s a strong choice for developing cloud native applications because of its scalability and its ability to manage concurrent users. The AngularJS frontend framework also makes it ideal for developing single-page applications that serve all information and functionality on a single page. Here are a few examples for using MEAN:**

* Calendars
* Expense tracking
* News aggregation sites
* Mapping and location finding

![image](https://user-images.githubusercontent.com/116161693/209232627-d1fa8f04-d2a7-48e2-b123-3253210becfa.png)

# In this Project we are going to deploy a web based application on our Linux server using MEAN STACK with the following steps.

if you have followed me on my first project on how to [implement LAMP](https://github.com/ChinenyenwaN1/DevOps-Project-Documentation/blob/main/LAMP-WEB%20STACK%20IMPLEMENTATION%20ON%20AWS.md/), you should be able to spin up a server and log into Mobaxterm app with your Keypair. 

# Step1: Install NodeJs

## Task
In this assignment you are going to implement a simple Book Register web form using MEAN stack.

## We will install nodejs on our server

Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

**Update ubuntu**

```
sudo apt update
```
![image](https://user-images.githubusercontent.com/116161693/209230363-aa17dc23-230b-4c9c-9ae0-34cd457719c2.png)

**Upgrade ubuntu**

```
sudo apt upgrade
```
**Lets get the location of Node.js software from Ubuntu repositories.**

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![image](https://user-images.githubusercontent.com/116161693/209230415-b5c2a174-f4eb-4679-8118-317d3a074280.png)

**Install NodeJS**

```
sudo apt install -y nodejs
```
![image](https://user-images.githubusercontent.com/116161693/209230492-4551ca3f-471f-4a70-8622-0cf4ebc68aa1.png)

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
![image](https://user-images.githubusercontent.com/116161693/209230911-1a6498fc-2da5-4264-8f30-a2f3483ed3be.png)

Install [npm](https://www.npmjs.com) – Node package manager.

```
sudo apt install -y npm
```

## Install ‘body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

```
sudo npm install body-parser
```

![image](https://user-images.githubusercontent.com/116161693/209230986-3c678d10-d5f1-4c3c-a8fc-46e4c660a5d2.png)

**Create a folder named ‘Books’**

```
mkdir Books && cd Books
```

**In the Books directory, Initialize npm project**

```
npm init
```

![image](https://user-images.githubusercontent.com/116161693/209231046-50d263b7-a035-4aa4-b097-424c23a8f5da.png)

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

![image](https://user-images.githubusercontent.com/116161693/209231134-c3d7a2a5-ca09-4dd2-bf5f-dd6f5ecad6f8.png)

![image](https://user-images.githubusercontent.com/116161693/209231319-d2e3a60a-6337-4ea5-a4f7-eb2139d41405.png)

# Step 3: Install Express and set up routes to the server

**Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.**

**We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.**

```
sudo npm install express mongoose
```
![image](https://user-images.githubusercontent.com/116161693/209231411-628a113d-0d5f-4f77-9f32-746f3d0b2af5.png)

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

![image](https://user-images.githubusercontent.com/116161693/209231473-c90cb165-678f-47f8-86c3-16edb1fda2b0.png)


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

![image](https://user-images.githubusercontent.com/116161693/209231520-486b7738-aa0e-4217-b36f-108e4d826844.png)


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

![image](https://user-images.githubusercontent.com/116161693/209231574-08c035eb-3cf4-4b75-b43a-80a4c0dd32a4.png)

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

![image](https://user-images.githubusercontent.com/116161693/209231599-096f6135-9fef-4dd0-84f0-90938feef1f6.png)

![image](https://user-images.githubusercontent.com/116161693/209231765-b6d5f619-486d-4d1d-9531-a134596e0e70.png)

**Change the directory back up to Books**

```
cd ..
```

**Start the server by running this command:**

```
node server.js
```

![image](https://user-images.githubusercontent.com/116161693/209231858-4a4dc18f-ff00-4388-86c6-d7903de653db.png)


## The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

```
curl -s http://localhost:3300
```


It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

Update the security group to open TCP port 3300 in your AWS Web Console for your EC2 Instance.

![image](https://user-images.githubusercontent.com/116161693/209232076-1c99b631-2ce9-4efd-b720-a425539ffe45.png)

Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

Quick reminder how to get your server’s Public IP and public DNS name:

You can find it in your AWS web console in EC2 details

![image](https://user-images.githubusercontent.com/116161693/209306762-04f7bc59-f232-4e9e-80b4-2e2ab1ed2065.png)


**Run curl -s http://169.254.169.254/latest/meta-data/public-ipv4 for Public IP address or curl -s http://169.254.169.254/latest/meta-data/public-hostname for Public DNS name.**

**Congratulations! This is how your Web Book Register Application will look like in browser:**




