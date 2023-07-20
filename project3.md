## MERN STACK IMPLEMENTATION ON AWS EC2 ##
`sudo apt update`
`sudo apt upgrade`
Get the location of Node.js software from Ubuntu repositories. 

`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/3ca26431-5f23-4395-a9fe-fb7c2ecc8b7d)

Install Node.js with the command below 

`sudo apt-get install -y nodejs`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/e88272dc-b877-4fcb-bab0-8861313a26de)

Verify the node installation with the command `node -v` and Verify the node installation with the command `npm -v`

Create a new directory for your To-Do project by running `mkdir Todo` and verify it was created ls

Initialize your project using npm init and run ls to verify you have package.json file created.

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/867819ae-b63b-4cd5-a42d-9d0d11d5d0a9)

INSTALL EXPRESSJS
To use express, install it using npm: npm install express

Create a file index.js: touch index.js and run ls to confirm it was created.

Install the dotenv module: npm install dotenv

Open the index.js file: vim index.js

Type the code below into it and save:

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

Start the server to see if it works. Open your terminal in the same directory as your index.js file and run: node index.js If every thing goes well, you should see Server running on port 5000 in your terminal.

On your EC2 instance, add an inbound rule to open port 5000.

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/a05bea4a-28e9-4f20-9a39-547688ea35c7)

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

`http://<PublicIP-or-PublicDNS>:5000`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/95364ed7-f582-450a-a585-a9f0733bff9c)

## MODELS
For the todo app to perform different tasks, we need to create api endpoints. This endpoints will use the POST, GET, and DELETE methods.

- Create routes folder and create an api.js file that is going to contain the api endpoints.
```
mkdir routes
touch api.js
```

- Edit api.js with the following code block:
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

Install mongoose which helps us connect to mongodb. 

`npm install mongoose --save`

![image](https://github.com/Itsjustfemi/cohort_learning/assets/98546783/b0e15a99-984d-477e-aa49-1a0ce736f585)

- Create models folder and create a todo.js file in it.
```
mkdir models
touch todo.js
```
Edit todo.js with the following code block:
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

- Update the api.js file in routes folder to include the following code which will make use of model:
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

### MONGODB DATABASE

- Create a modngoDB database on mongodb.com, create a user, allow connection anywhere and also create a collection called todo.

![](./images/p3/ScreenShot_4_6_2022_11_44_22_PM.png)

- Create a .env file in the root directory of the project and add the following code:
```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```

![](./images/p3/ScreenShot_4_6_2022_11_53_24_PM.png)

- Edit the index.js file to include the following code:
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
- Run the server by running the command `node index.js`

![](./images/p3/ScreenShot_4_11_2022_1_23_54_PM.png)

As you can see from tht above image, i had some issues running the server, after some troubleshooting, i discovered i had to kill the current running process on port 5000 for the new one to begin. This i did by first getting the Process Id(PID) for the current process `lsof -i tcp:5000` 

After i got the PID, i killed it using the kill command:

`kill -9 "<the PID>"`

- The images bellow show the use of POST method to query the database.

![](./images/p3/ScreenShot_4_11_2022_3_36_59_PM.png)
