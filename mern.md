# DEPLOYING A MERN STACK
**Prerequisites**

We need access to an Ubuntu 20.04 server

*Step 1 – Backend configuration*

Update ubuntu

`sudo apt update`

Upgrade ubuntu

`sudo apt upgrade`

![ubuntu upgrade and update](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern1.JPG)

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern2.JPG)

Get the location of Node.js software from Ubuntu repositories.

`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern3.JPG)

*Install Node.js on the server* 

Install Node.js with the command

`sudo apt-get install -y nodejs`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern4.JPG)


Note: The command above installs both nodejs and npm.

Verify the node and npm installation with the command below

`node -v `

`npm -v`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern5.JPG)


*Application Code Setup*

Create a new directory for your To-Do project:

`$ mkdir Todo`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern6.JPG)


verify that the `Todo` directory is created with 

`$ ls`


Now change your current directory to the newly created one:

`$ cd Todo`

Next, you will use the command `npm init` to initialise your project, so that a new file named `package.json` will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press `Enter` several times to accept default values, then accept to write out the `package.json` file by typing yes.

![npm init](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern7.JPG)

*Install ExpressJS*

Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:

`$ npm install express`

![install expess](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern8.JPG)

Now create a file index.js

![new file](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern9.JPG)


Run `ls` to confirm that your `index.js` file is successfully created

Install the `dotenv` module

`npm install dotenv`

![dotenv](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern10.JPG)

Open the index.js file 

`$ vim index.js`

Type the code below into it and save.

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

![index.js](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern11.JPG)


Notice that we have specified to use `port 5000` in the code. This will be required later when we go on the browser.

Use` :w` to save in vim and use `:qa` to exit vim

Now it is time to start our server to see if it works. Open your terminal in the same directory as your `index.js` file and type:


`$ node index.js`

If every thing goes well, you should see Server running on port 5000 in your terminal.

![server running](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern12.JPG)

NB:ensure that firewall is enabled by  running
`ufw enable`
and also, make sure `port 5000 `is opened run

`sudo ufw allow 5000`


*Routes*

There are three actions that our To-Do application needs to be able to do:

Create a new task
Display list of all tasks
Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create `routes` that will define various endpoints that the `To-do` app will depend on. So let us create a folder `routes`

`$ mkdir routes`

![server running](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern13.JPG)



Change directory to `routes` folder.

`$ cd routes`

Now, create a file `api.js` with the command below

`touch api.js`

Open the file with the command below

`vim api.js`

Copy below code in the file. 

const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

*Models*

Since the app is going to make use of `Mongodb` which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each `Mongodb` document

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, install `mongoose` which is a `Node.js` package that makes working with `mongodb` easier.

Change directory back `Todo` folder with `cd ..` and install `Mongoose`

`$ npm install mongoose`


![server running](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern14.JPG)


Create a new folder with `mkdir models` command

Change directory into the newly created ‘models’ folder with `cd models`.

Inside the models folder, create a file and name it `todo.js`

`touch todo.js`

Open the file created with `vim todo.js` then paste the code below in the file:


![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern15.JPG)

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern16.JPG)

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


Now we need to update our routes from the file `api.js` in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with `vim api.js`, delete the code inside with `:%d `command and paste there code below into it then save and exit

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

*MongoDB Database*

We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), you will need to sign up for a shared clusters free account, which is ideal for our use case.select AWS as the cloud provider, and choose a region near you.

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern17.JPG)

Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

IMPORTANT NOTE In the image below, make sure you change the time of deleting the entry from 6 Hours to 1 Week



![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern18.JPG)

Create a MongoDB database and collection inside mLab

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern19.JPG)


In the `index.js `file, we specified `process.env` to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your Todo directory and name it `.env`.

`touch .env`

`vi .env`

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern20.JPG)

Add the connection string to access the database in it, just as below:

DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'

Ensure to update <username>, <password>, <network-address> and <database> according to your setup

Here is how to get your connection string

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern21.JPG)

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern22.JPG)


Now we need to update the `index.js` to reflect the use of `.env `so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

To do that using vim, follow below steps

Open the file with `vim index.js`
Press `esc`
Type `:`
Type `%d`
Hit ‘Enter’
The entire content will be deleted, then,

Press i to enter the insert mode in vim
Now, paste the entire code below in the file.


const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true })
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

Start your server using the command:

`node index.js`

You shall see a message ‘Database connected successfully’, if so - we have our backend configured. Now we are going to test it

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern23.JPG)

*Testing Backend Code without Frontend using RESTful API*

So far we have written backend part of our `To-Do` application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use Postman to test our API. 

You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.

*Step 2 - Frontend creation*

To start out with the frontend of the `To-do `app, we will use the `create-react-app` command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run:

`$ npx create-react-app client`
![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/Screen%20Shot%202021-03-20%20at%2010.50.56%20PM.png)

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/Screen%20Shot%202021-03-20%20at%2010.52.20%20PM.png)

This will create a new folder in your `Todo` directory called client, where you will add all the react code.

*Running a React App*

Before testing the react app, there are some dependencies that need to be installed.

Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

`$ npm install concurrently --save-dev`

Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`$ npm install nodemon --save-dev`

In Todo folder open the `package.json` file. Change the highlighted part of the below screenshot and replace with the code below

"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},

![](https://github.com/olateekay/deploying-mern-stack/blob/main/images/Screen%20Shot%202021-03-20%20at%2010.55.29%20PM.png)





