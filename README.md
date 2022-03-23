# Api-reference




Culture
What we do
Who we Worked with
About Us
Technology
Careers
Works @ Bacancy
Blogs
Resources
Testimonial
Contact
Get Quote
IT Blog | Mobile App Development India | Offshore Web Development - Bacancytechnology.com
Type your search query and hit enter:
What are you looking for?
Node.js
API Request Schema With Joi Validation in NodeJS and Express
Assume you’re working on an endpoint that expects user data such as – username, age, address, pin code, state, and phone number. Consider a user entering numeric data for the username by mistake when you’re expecting alphabetic data; a user entering an invalid pin code or birthdate for their respective fields when you’re expecting the data in a particular format. You don’t wish to make undesirable data! So, you can go for Data Validation to make sure that the data you receive is in a proper format. And for validating your data, you can either choose a manual way of coding the whole logic or use a validation library/package.

In this tutorial, we will learn about how to use joi validation in node js and Express. For your better understanding, I have divided the entire tutorial of joi validation in node js example into various sections.

What is Joi?
Joi is the most famous, efficient, and widely used package for object schema descriptions and validation. Joi allows the developers to build the Javascript blueprints and make sure that the application accepts the accurately formatted data.

Here are some of the advantages of Joi-

Easy to implement and easy to learn.
Widely accepted and well-known package for data validation.
Supports validation based on the schema.
Let us start with creating a simple demo application for joi validation in node js and joi express validation.

Steps to implement API Request Schema with Joi Validation In Node.js and Express.
Initial Setup
Create a directory that is our root directory

mkdir JoiValidation
cd JoiValidation
Define package.json file
For creating one, run this command:

npm init
Install dependencies
We have five dependencies to be installed for Joi file Validation:

bcryptjs
body-parser
dotenv
express
joi
mongoose
Run this command to install all the dependencies mentioned above.

npm install --save bcryptjs dotenv express joi mongoose
1. bcryptjs: It is used for storing plaintext password into hashing password for security purposes.

2. dotenv: A module with no dependency. It is used for loading the environment variables from a .env file into the process .env.

3. express: It is a well-known and flexible Node.js framework for developing web applications. It offers robust features for building web/mobile applications.

4. joi: It is an object schema description language that validates JS objects. With Joi, we can quickly build schemas for JS objects and make sure to validate critical data.

5. mongoose: It is a tool for MongoDB object modelling, which is designed to work in an async environment.

Create Server and Connect with Database
Creating a .env file in the root directory for storing our environment variables.

In the .env file, we need to create two variables – the first is the PORT, and another is MONGO_URI, for your database connection.
Then, provide appropriate value for both the variables.

Once you’re done with that, create a folder src in the root directory. It will contain various folders like controllers, models, db, routes and utils. We will cover on later.

Creating Connection file
In the db folder, we will create a conn.js file and write code for database connection.

conn.js

const mongoose = require('mongoose');

mongoose.connect(process.env.MONGO_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true
}).then(() => {
    console.log('Connection successful!');
}).catch((e) => {
    console.log('Connection failed!');
})
In conn.js, we need to require a mongoose package to connect with the MongoDB database.
connect() function takes two arguments – first is uri, and another is options.
We will get MONGO_URI from the .env file.

Creating server file
Create a server.js file in the src folder

server.js

require("dotenv").config();
require('./db/conn');
const express = require("express");

const app = express();
const port = process.env.PORT;

app.use(express.json({ limit: "10MB" }));
app.use(express.urlencoded({ extended: true }));

app.listen(port, () => {
console.log(`Server is running at ${port}`);
});
Once we are done with the initialising of const express, we will further use json() and urlencoded() to parse JSON data from the request body and urlencoded bodies. Now, we will check whether our connection is established or not.
To run the server, we have to add two scripts in the package.json file.
package.json

"dev": "nodemon src/server.js",
"start": "node src/server.js"
dev – for pre-installed nodemon in your system.
start – for running server without nodemon.
Test your connection by running this command:

npm start
The message displayed in your terminal is given below.

Server is running at 3000
Connection successful!

So, the connection has been established successfully!

Creating Home Route
Inside the utils folder, we will create an errorFunction.js file for creating errorFunction() to display error messages and data in JSON format.
errorFunction.js

const errorFunction = (errorBit, msg, data) => {
     if (errorBit) return { is_error: errorBit, message: msg };
     else return { is_error: errorBit, message: msg, data };
};

module.exports = errorFunction;
errorFunction() takes three arguments –
1. errorBit – for checking if the error has occurred or not,
2. msg – for displaying appropriate messages for which action is performed,
3. data– sent to the user.

Whenever an error occurs, it will return – errorBit and msg or else errorBit, msg and data.
Whenever an error occurs, it will return – errorBit and msg or else errorBit, msg and data.

Creating controller
In controllers, we need to create the defaultController.js file.
defaultController.js

const errorFunction = require("./../utils/errorFunction");

const defaultController = async (req, res, next) => {
res.status(200);
res.json(errorFunction(false, "Home Page", "Welcome from Bacancy"));
};

module.exports = defaultController;
Create a defaultController variable for sending a JSON response to the user. It sends three arguments –
1. false will be the errorBit
2. “Home Page” will be a message
3. “Welcome from Bacancy” will be data.

And then export the defaultController variable.

Creating Route
Inside the routes folder, we need to create the userRoute.js file

userRoute.js

const express = require("express");
const defaultController = require("../controllers/defaultController");

const router = express.Router();

router.get("/", defaultController);

module.exports = router;
Create a router variable for creating get or post requests. Here, we will create a get request for a home route.

In the server.js file, add the following lines to use the router.

const userRoutes = require("./routes/userRoutes");
app.use('/', userRoutes);
Testing Home API in Postman
Creating User Model
Now, it’s time to create a user model for storing user schema. Inside the models folder, create a user.js file.
user.js

const mongoose = require("mongoose");
const userSchema = new mongoose.Schema(
{
 userName: {
  type: String,
  required: true,
  trim: true,
 },


 email: {
  type: String,
  required: true,
  trim: true,
 },
 password: {
  type: String,
  required: true,
  trim: true,
 },
 mobileNumber: {
  type: String,
  maxlength: 10,
  required: true,
 },
 birthYear: {
  type: Number,
  max: 2000,
  min: 1900,
 },
 skillSet: {
  type: Array,
  default: [],
 },
 is_active: {
  type: Boolean,
  default: true,
 },
},
{ timestamps: true }
);

const User = mongoose.model("user", userSchema);

module.exports = User;
We require a mongoose package to write a user schema, a model() method to create a user model into our database.

We require a mongoose package to write a user schema, a model() method to create a user model into our database. The model() method, “user,” will be the model name and userSchema will be our userSchema variable. For storing hashing passwords into the database, we will create a securePassword() function. In the utils folder, create securePassword.js file.

securePassword.js

const bcrypt = require("bcryptjs");

const securePassword = async (password) => {
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(password,  salt);
    return hashedPassword;
};
module.exports = securePassword;
Here, we will use the bcryptjs package for hashing the password.
Create securePassword() function for password hashing. It takes one argument, which is a plaintext password. This function will return a hashed password.

Now, we need to create user payload validation using the joi package.
Create a user folder into the controllers folder. Inside the user folder, we need to create user.validator.js for user payload validation.

user.validator.js

const joi = require("joi");
const errorFunction = require("../../utils/errorFunction");

const validation = joi.object({
     userName: joi.string().alphanum().min(3).max(25).trim(true).required(),
     email: joi.string().email().trim(true).required(),
     password: joi.string().min(8).trim(true).required(),
     mobileNumber: joi.string().length(10).pattern(/[6-9]{1}[0-9]{9}/).required(),
     birthYear: joi.number().integer().min(1920).max(2000),
     skillSet: joi.array().items(joi.string().alphanum().trim(true))
.default([]),
    is_active: joi.boolean().default(true),
});
In user.validator.js we require the joi package for payload validation and errorFunction() for JSON response. To create schema object validation, we will use the object() function of joi. We have various properties to validate. Each property needs joi chaining function to validate.

Here are some of the joi functions which we used to validate property.

1. Joi.string(): It validates that all properties are string
2. Joi.alphanum(): It must contain only alphanumeric characters.
3. Joi.min(): It specifies the minimum number.
4. Joi.max(): It specifies the maximum number.
5. Joi.trim(): Requires the string value to contain no whitespace before or after.
6. Joi.required(): Make a property required which will not allow undefined as value.
7. Joi.email(): It validates that the email property contains a valid email address.
8. Joi.length(): It specifies the exact number of character or number of items in the array.
9. Joi.pattern(): A pattern will be a regular expression. It specifies validation rules for matching a pattern.
10. Joi.integer(): Requires the number to be an integer (no floating point).
11. Joi.array(): Generates a schema object that matches an array data type.
12. Joi.items(): Lists the types allowed for the array values were types – one or more joi schema objects to validate each array item against.
13. Joi.default(): creates a default value for property.
14. Joi.boolean(): Generates a schema object that matches a boolean data type (as well as the strings ‘true’, ‘false’, ‘yes’, and ‘no’).

After that, we will create a middleware function for validating users payload.

const userValidation = async (req, res, next) => {
 const payload = {
  userName: req.body.userName,
  email: req.body.email,
  password: req.body.password,
  mobileNumber: req.body.mobileNumber,
  birthYear: req.body.birthYear,
  skillSet: req.body.skillSet,
  is_active: req.body.is_active,
 };

 const { error } = validation.validate(payload);
 if (error) {
  res.status(406);
  return res.json(
   errorFunction(true, `Error in User Data : ${error.message}`)
  );
 } else {
  next();
 }
};
module.exports = userValidation;
Here userValidation is a middleware to validate users payload. We will create a payload variable for storing request data. Now, use validate() function to validate users’ payload.

If any error occurred while validating the payload, it displays an error message with status code 406. If no error occurred, then it calls the next() function.

Adding user into the database
Write the following code in the user.controller.js file inside the user folder.

user.controller.js

const User = require("../../models/user");
const errorFunction = require("../../utils/errorFunction");
const securePassword = require("./../../utils/securePassword");

const addUser = async (req, res, next) => {
 try {
  const existingUser = await User.findOne({
   email: req.body.email,
  }).lean(true);
  if (existingUser) {
   res.status(403);
   return res.json(errorFunction(true, "User Already Exists"));
   } else {
    const hashedPassword = await securePassword(req.body.password);
    const newUser = await User.create({
     userName: req.body.userName,
     email: req.body.email,
     password: hashedPassword,
     mobileNumber: req.body.mobileNumber,
     birthYear: req.body.birthYear,
     skillSet: req.body.skillSet,
     is_active: req.body.is_active,
   });
   if (newUser) {
    res.status(201);
    return res.json(
     errorFunction(false, "User Created", newUser)
    );
   } else {
    res.status(403);
    return res.json(errorFunction(true, "Error Creating User"));
   }
  }
 } catch (error) {
  res.status(400);
  console.log(error);
  return res.json(errorFunction(true, "Error Adding user"));
 }
};
We need a User model to add data into the database, errorFunction(), and securePassword().

Inside the addUser() function, first, we need to check if the user already exists or not. If a user exists then, it will display an error message; otherwise, move further.
Now, use the create() function to insert data into the database.

If a user is created, it will display a “user created” message or throw an error message.

Creating post request for add User
To create a post request, we need to require userValidation middleware and addUser function.
userRoute.js

const userValidation = require("../controllers/user/user.validator");

const { addUser } = require("../controllers/user/user.controller");

router.post("/addUser", userValidation, addUser);
Testing AddUser API in Postman :
Fetching Users from Database
user.controller.js

const getUsers = async (req, res, next) => {
 try {
  const allUsers = await User.find();
  if (allUsers) {
   res.status(201);
   return res.json(
    errorFunction(false, "Sending all users", allUsers)
   );
  } else {
   res.status(403);
   return res.json(errorFunction(true, "Error getting Users"));
  }
 } catch (error) {
  res.status(400);
  return res.json(errorFunction(true, "Error getting user"));
 }
};

module.exports = { addUser, getUsers };
To fetch all the users from the database, we have to use the find() method of the User model. If users exist in the database, then it will display the users or else error messages.Once done export the getUsers() function.

Creating get a request to fetch Users
To create get a request, we will use the getUsers function.

userRoute.js

const { addUser, getUsers } = require("../controllers/user/user.controller");

router.get("/getUsers", getUsers);
Testing getUsers API in Postman :
Here’s the source code of the demo application – Github Repository

After creating all the folders and files, the final project structure will look something like this-


Conclusion
So, this was all about implementing API Request Schema with express Joi Validation Node.js. I hope the tutorial of Node js joi validation example was helpful. If you wish to learn more about Joi NodeJS, please feel free to visit NodeJS Tutorials without wasting a minute to start coding us!

Bacancy Technology has efficient, skilled, and dedicated NodeJS developers having fundamental and advanced back-end knowledge. If you are looking for such skilled developers, then contact Bacancy Technology and hire Node developer.

Last Updated on February 10, 2022

PUBLISHED BY
Archita Nayak and Happy Bhesdadiya
MAY 8, 2021
All Rights ReservedView Non-AMP Version
t
