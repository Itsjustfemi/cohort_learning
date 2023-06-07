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

Create routes folder and create an api.js file that is going to contain the api endpoints.

`mkdir routes`

touch api.js

Edit api.js with the following code block:
```const express = require ('express');
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

