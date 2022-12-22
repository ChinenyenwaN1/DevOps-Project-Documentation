# Mern.Project
MERN
# Mern-stack-project-

## MERN Stack

### MERN  stack is a web development framework. It consists of MongoDB, ExpressJS, ReactJS, and NodeJS as its working components. 

Each of these 4 powerful technologies provides an end-to-end framework for the developers to work in and each of these technologies play a big part in the development of web applications.

Here are the details of what each of these components is used for in developing a web application when using MERN stack:

* MongoDB: A document-oriented, No-SQL database used to store the application data.

* NodeJS: The JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

* ExpressJS: A framework layered on top of NodeJS, used to build the backend of a site using NodeJS functions and structures. Since NodeJS was not developed to make websites but rather run JavaScript on a machine, ExpressJS was developed.

* ReactJS: A library created by Facebook. It is used to build UI components that create the user interface of the single page web application.

### Understanding the Parts of the MERN Stack

## MongoDB

 * MongoDB is a cross-platform document-based database known as a nonrelational document-orientated database, or NoSQL. MongoDB stores data in flexible documents using a query language based on JavaScript Object Notation (JSON). The size, number and content of document fields vary, so the data structure can be altered later on. MongoDB is easy to scale and quite flexible, making it popular among many web developers.

## Express

 * Express is a back end framework for Node.js web applications. We can use Express to make writing server code much simpler, as opposed to the traditional alternative of manually writing full web server code on Node.js, since it lets us avoid having to repeat the same code like we would using the Node.js HTTP module.
Express’s framework was designed to enable simple, easy construction of APIs and efficient web applications. It is loaded with plugin features and is renowned for its speed. Its minimalistic structure makes it simple for developers to pick up and start using right away.

## React

 * In React, views are declarative, meaning developers do not have to manage the effects of changes in data or the view’s state. (A view’s state is the object that determines the behavior of individual components.) Instead, React lets developers use a full-featured programming language to design conditional or repetitive DOM elements rather than relying on templates that automatically create repetitive DOM (Document Object Model) or HTML elements. React lets the same code run on the browser and server simultaneously, which is what enables React to anchor the MERN stack and makes it essential to the operation.

## Node.js

 * Node.js is a cross-platform JavaScript runtime environment that is built on Chrome’s V8 JavaScript engine and is intended to allow developers to build scalable network applications and execute JavaScript code outside of browsers. Node.js works by using its own module system that is based on CommonJS, meaning it does not need an enclosing HTML page to put together more than one JavaScript file.

How does the MERN stack work?

The MERN architecture allows us to easily construct a 3-tier architecture (frontend, backend, database) entirely using JavaScript and JSON.

![image](https://user-images.githubusercontent.com/116161693/209102106-63d39a33-7108-4efa-9339-60d912883f87.png)

if you have followed me on my first project on how to [implement LAMP](https://github.com/ChinenyenwaN1/DevOps-Project-Documentation/blob/main/LAMP-WEB%20STACK%20IMPLEMENTATION%20ON%20AWS.md/), you should be able to spin up a server and log into Mobaxterm app with your Keypair. 

**In this project, we are required to implement a web solution based on MERN stack on a Linux server. We will be deploying an application that is used to create ‘Todo’ lists by implementing the steps below:

# Step 1 - Prepare the backend Linux server

* Update and upgrade ubuntu

```
sudo apt update
sudo apt upgrade
```

# Step 2 - First, we will need to locate and install nodejs on the server.

**Lets get the location of Node.js software from Ubuntu repositories.**

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

# Install Node.js on the server
Install Node.js with the command below

```
sudo apt-get install -y nodejs
```

**Note:** The command above will install both nodejs and npm. NPM is a package manager. Similar to what apt does on Ubuntu, NPM is used to install node modules or softwares, and manages dependency conflicts intelligently.


 * Verify the node installation with both commands below
 
```
node -v
 
npm -v 
```

![image](https://user-images.githubusercontent.com/116161693/209078621-11595d68-ed72-4fca-846f-83054115ce5a.png)

# Step 3 - Application Code Set
 
 
 **Create a new directory for your To-Do project:
 
 
```
mkdir Todo
```
 
 
 **Verify that the Todo directory is created


```
ls
```

**Now change your current directory to the newly created one:**


```
cd Todo
```
![image](https://user-images.githubusercontent.com/116161693/209078871-db0175fe-5c74-49a7-8ead-6201561d9ecc.png)

**Next, we will use the command npm init to initialise our project, so that a new file named package.json will be created. This file will normally contain information about our app and the dependencies that it needs to run. Follow the prompts after running the command. We can hit enter to accept default values, then we accept to write out the package.json file by typing yes.
 
```
npm init
```
 
 ## We'll see something like this
 
 ![image](https://user-images.githubusercontent.com/116161693/209078983-08d8c836-4053-445e-87af-776936dbdf10.png)
 
 **Run the command ls to confirm that you have package.json file created.
 
# Step 4 - Install ExpressJS

## Express is a back end framework for Node.js web applications. it lets us avoid having to repeat the same code like we would using the Node.js HTTP module and It also helps to define routes of our application based on HTTP methods and URLs.

**Inside the todo list folder,We will install Express using npm :**

```
npm install express
```

**Now create a file index.js with the command below**

```
touch index.js
```

**Run ls to confirm that your index.js file is successfully created**

```
ls
```


**Inside the todo list folder, We will Install the dotenv module**

```
npm install dotenv
```

**We need to open the index.js file with the command below**

```
vi index.js
```

**Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.**

```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```


**We'll see something like this

![image](https://user-images.githubusercontent.com/116161693/209079491-1752e121-cf5a-4b18-a9dc-c80be8b61988.png)

We use :w to save in vi and use :q! to exit vi.


NOTE we have specified to use port 5000 in the code. This will be required later when we go on the browser.


To test if our server works. We open our terminal in the same Todo directory as our index.js file and type

```
node index.js
```

**If every thing goes well, We should see our Server running on port 5000 in our terminal.**

![image](https://user-images.githubusercontent.com/116161693/209079222-5a592de2-0631-4932-8bf3-e08e7da9dd1a.png)

Now we need to open this port in EC2 Security Groups. Refer to Project 1 Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000, like this:

![image](https://user-images.githubusercontent.com/116161693/209091687-99e7c0ae-d7c3-4b7c-a42c-96af4d37a0d6.png)

```
http://<PublicIP-or-PublicDNS>:5000
```
  
Quick reminder how to get your server’s Public IP and public DNS name:
  
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
curl -s http://169.254.169.254/latest/meta-data/public-hostname
```

![image](https://user-images.githubusercontent.com/116161693/209100896-c21b2839-774e-4236-9140-723fbbbef733.png)
  
## Step 5 - Routes

**For each task, we need to create routes that will define various endpoints that the todo app will depend on.**

So let’s create a folder routes
 
 ```
mkdir routes
 ```
 
Change directory into the routes folder.
 
```
cd routes
```

Now, create a file api.js with the command below
 
```
touch api.js
```

 
Open the file with the command below

 
```
vi api.js
```
![image](https://user-images.githubusercontent.com/116161693/209092095-8bcbb9eb-00b2-449a-a273-07d4dc339618.png)

* Input below code in it.

```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

## Step 6 - Models

Since the app is going to make use of Mongodb which is a NoSql database, we need to create a model which will be used to define the database schema. This is important so that we will be able to define the fields stored in each Mongodb document.
 
 
In essence the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties. To create a Schema and a model, install mongoose which is a node package that makes working with mongodb easier.
 
Change directory back Todo folder with cd .. and install Mongoose

```
npm install mongoose
```
![image](https://user-images.githubusercontent.com/116161693/209092218-9d6c9d7b-e9f9-44f1-8671-e1102ca0a73e.png)

Create a new folder with mkdir models command
 
```
mkdir models
```
 
Change directory into the newly created ‘models’ folder with
 
```
cd models
```
 
Inside the models folder, create a file and name it
 
```
touch todo.js
```
 
<img width="435" alt="mkdir" src="https://user-images.githubusercontent.com/85270361/128392550-4aaa89e5-e3b6-4e41-9a17-345f2fa5f1c8.PNG">
 
All three commands above can be defined in one line to be executed consequently with help of && operator, like this:
 
```
mkdir models && cd models && touch todo.js
```

Open the file created with vi todo.js then paste the code below in the file:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
 
![image](https://user-images.githubusercontent.com/116161693/209093095-f3660397-ca0d-4313-bc5c-1bdd106a7542.png)

Now we need to update our routes from the file api.js in Routes directory to make use of the new model.


In Routes directory, open api.js with vi api.js, delete the code inside with :%d and paste the code below into it then save and exit


```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
```
![image](https://user-images.githubusercontent.com/116161693/209093247-f3d24664-78cb-4f8e-b6ea-9536ca43fb48.png)

# Step 7- MongoDB Database
 
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case

https://www.mongodb.com/atlas-signup-from-mlab 
Follow the sign up process, select AWS as the cloud provider, and choose a region near you.


Create a mongodb database and connection inside mLab and then obtain the connection string through the connect TAB. Also, update the password and database name and removed the <>.


DB = 'mongodb+srv://ChinenyeDB:password@cluster0.2maews4.mongodb.net/?retryWrites=true&w=majority'
 
 
Create a file in your TODO directory with and name it .env. Add the connection string to access the database in it, just as below
 
 
In order to allow the node.js to connect to the database in Mongodb, the index.js was updated so as to reflect the use of dotenv.
 
```
touch .env
vi .env
```

  
Create a mongodb database and connection inside mLab and then obtain the connection string through the connect TAB. Also, update the password and database name and removed the <>.
 
 
Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

To do that using vim, follow below steps

Open the file with vim index.js
 
Press esc
 
Type :
 
Type %d
 
Hit ‘Enter’
 
The entire content will be deleted, then,

Press i to enter the insert mode in vim
 
Now, paste the entire code below in the file.
 
  
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

![image](https://user-images.githubusercontent.com/116161693/209093764-177af9ab-5af1-4470-a1e2-55a5ca2bb9ab.png)

 
Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

Start your server using the command:
 
```
node index.js
```
 
![image](https://user-images.githubusercontent.com/116161693/209094070-a59edf9f-7d15-4129-a5e3-d3c3d3337392.png)

![image](https://user-images.githubusercontent.com/116161693/209094155-5e9b56f5-d7e6-493f-99b5-a8f176e4a5df.png)
 
# Step 8 -Testing Backend Code without Frontend using RESTful API
 
So far we have written our TODO application, and configured backend database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTFUL API. Therefore, we will need to make use of some api development clients to test our code.
 
In this project, we will use Postman to test for our API.
 
Install Postman in Ubuntu
 

```
sudo snap install postman
```
 
![image](https://user-images.githubusercontent.com/116161693/209094337-fa0adc91-eae2-422b-ad97-46e0cd402e8e.png)
 
Now we open our Postman, We will create a ‘POST’ and ‘GET’ request in postman to the API http://localhost:5000/api/todos
 
POST Request
 
![image](https://user-images.githubusercontent.com/116161693/209094378-80a343b8-6f96-4c38-8ba2-7a7d3fd3be27.png)
 
# GET Request

![image](https://user-images.githubusercontent.com/116161693/209095160-b10cf4c5-6d24-4ede-8b59-53e1937f2f77.png)
 
# Step 9 - Creating the Frontend
 
**we are done with the functionality we want from our api, it is time to create an interface for the client to interact with the api. To start out with the frontend of the todo app, we will use the create-react-app command to scaffold our app.
 
 
* In our Todo directory, We will run:
 
```
npx create-react-app client
```
 
This will create a new folder in our Todo directory called client, where we will add all the react code.
 
![image](https://user-images.githubusercontent.com/116161693/209095479-fc337944-27cc-4c07-b2fe-7c57afd59816.png)
 
##Step 10 - Running the React App
 
**Before testing the react app, there are a number of dependencies that need to be installed.
 
We need to Install concurrently: It is used to run more than one command simultaneously from the same terminal window.
 

```
npm install concurrently --save-dev
```
 
![image](https://user-images.githubusercontent.com/116161693/209095554-0b42385c-e7cf-49d7-a29e-1f3af6ef94ea.png)
 
* We need to Install nodemon: It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
 
```
npm install nodemon --save-dev
```
 
![image](https://user-images.githubusercontent.com/116161693/209095586-f8e17e12-0a24-4457-8028-27da3d4452a7.png)
 
* In the Todo folder, open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.

 
```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

![image](https://user-images.githubusercontent.com/116161693/209095688-cd5c65f5-641a-4154-a572-49558040afb6.png)

# Step 11 - Configure Proxy in package.json
 
* Enter into the client folder from Todo directory

```
cd client
```
 
* Open the package.json file
 
```
vi package.json
```
 
* Add the key value pair in the package.json file "proxy": "http://localhost:5000".
 
![image](https://user-images.githubusercontent.com/116161693/209095769-c3f31415-3faf-464a-8c3a-824cff8d29fa.png)
 
The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos
 
# Now, we will ensure that we are inside the Todo directory, and simply do:
  
```
npm run dev
```

![image](https://user-images.githubusercontent.com/116161693/209095976-2e188fee-38fb-4652-ad5b-6ba4b777796a.png)

![image](https://user-images.githubusercontent.com/116161693/209096462-bbfe1cdd-05f6-450d-9372-eecc4cc57c39.png)
 
**Your app should open and start running on localhost:3000**

![image](https://user-images.githubusercontent.com/116161693/209096612-906d2fa4-0716-45a6-b4f0-562d1453c913.png)
 
Important note: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.
 
# Step 12 - Creating our React Components 
 
Creating your React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component. From your Todo directory run
 
 
```
cd client
```
 
**Move to the src directory**
 
```
cd src
```
 
**Inside your src folder create another folder called components**
 
 
```
mkdir components
```

**Move into the components directory with** 

 ```
 cd components
 ```
 
Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.
 
**touch Input.js ListTodo.js Todo.js**
 
![image](https://user-images.githubusercontent.com/116161693/209096728-74b2328a-6fd9-4e38-bdb3-81faf1c96763.png)

Open Input.js file

```
vi Input.js
```
 
**Copy and paste the following**

```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```

 
Axios is a promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.
 
 
* Move to the src folder
 
```
cd ..
```
 
* Move to clients folder
 
```
cd ..
```
 
 
**Install Axios in the clients folder**
 
```
npm install axios
```
 
![image](https://user-images.githubusercontent.com/116161693/209096848-fe8b4d25-e933-45d1-aadf-e3f56f072178.png)

* Go to components directory
 
```
cd src/components
```
 
 
* After that open your ListTodo.js
 
```
vi ListTodo.js
```

* In the ListTodo.js copy and paste the following code
 
```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
```
 
* **Then in your Todo.js file you write the following code**
 
```
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```
 


**We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.**

* Move to the src folder

 
```
cd ..
```
 
 
* **Make sure that you are in the src folder and run**

 
```
vi App.js
```
 
 
**Copy and paste the code below into it**
 
 
``` 
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
```
 
 
**After pasting, exit the editor.**

* In the src directory open the App.css 
 
 
```
vi App.css
```
 
 
**Then paste the following code into App.css:**
 
 
```
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
```
 
 
 
## Exit
 

* In the src directory open the index.css

 
```
vim index.css
```
 
 
**Copy and paste the code below:**
 
 
```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```

 
**Go to the Todo directory**

 
```
cd ../..
```
 

When you are in the Todo directory run:


```
npm run dev
```

![image](https://user-images.githubusercontent.com/116161693/209097426-64c8758f-be46-4433-b6d7-98ec727f8662.png)
 
 
Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.
 
![image](https://user-images.githubusercontent.com/116161693/209097326-62069931-4097-471b-b41a-7435c38ec0cd.png)

**Congratulations! our To-Do Application is up and running.**

