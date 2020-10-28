# Mongoose Schemas + Models

This assignment will give you chance to practise

- using a .env file
- creating express endpoints
- writing Mongoose Schemas + Models

## Assignments

## Assignment 1 - Write a .env file

Note: For this assignment, we already assume you have a MongoDB server, and know how to access your credentials.

1. Create a file in your root folder called `.env`. This file will contain all the connection information for accessing your database.

2. Copy and paste the text in the file `.env.example`, into your `.env` file.

3. For the key `DB_NAME`, choose anything you like. This will be the name of the database we will use (we will only use it for this project). All data we save in this assignment will go inside this database.

    > Hint: The `DB_NAME` could be related to this assignment, for example `MONGOOSE_SCHEMA_ASSIGNMENT`

4. For the other keys, fill in the details as provided to you by your MongoDB service.

    - DB_HOST=
    - DB_USER=
    - DB_PASS=

## Assignment 2 - Using dotenv

1. Use the following command to install the `dotenv` module

    ```shell script
    npm install dotenv
    ```
   
2. Inside `server.js`, import the `dotenv` module with the following code:

    ```javascript
    const dotenv = require('dotenv');
    
    dotenv.config();
    ```

3. Test that your variables are being passed into your application through with `console.log`. For example, for the first variable, we can use:

    ```javascript
    console.log(process.env.DB_NAME);
    ```

## Assignment 3 - Connecting Mongoose with your database

We will install `mongoose` and connect it to our database

1. Install `mongoose`

    ```shell script
    npm install 
    ``` 
   
2. Import `mongoose` into `server.js` using the `require` function

3. Use the `connect` method from `mongoose` to connect to your database

    ```javascript
     mongoose.connect()
    ```
   
4. The `connect` method needs the URL of your database to connect to (as a string). Pass into the `connect` method all the correct parameters which should now be accessible to you via the `process.env` global variable. You can use the following code as an example (you will have to fill in the correct parameters as marked by {} from your own `process.env` properties):

    ```javascript
    mongoose
        .connect(
            'mongodb+srv://{DB_USER}:{DB_PASS}@{DB_HOST}/{DB_NAME}?retryWrites=true&w=majority')
    ```
   
## Assignment 4 - Let's test our DB connection

The `mongoose.connect()` method returns a promise, which we can use to determine if the connection with the database went ok or not.

1. Knowing this, use the `then` promise method (meaning the promise was resolved) to display a message that everything went ok, in our `console`

2. Use the `catch` promise method (meaning the promise was rejected) to display a message that connecting to our database failed, in our `console`

3. Test your code by running it. What message do you see in the `console`? 

## Assignment 5 - Creating a registration endpoint

Now we've connected our database, we want to build an endpoint which will allow us to register new users onto our website. To do this, we will have to use `express.js`

1. Install express

    ```shell script
    npm install express
    ```
   
2. Import `express` into `server.js` using the `require` function

3. Create an endpoint inside `server.js`. It should:
    - listen for a `POST` request (we want to receive data)
    - use the path `/user/register`
    - don't forget `response.send()`

4. Make a call to `app.listen()` to allow the server to start listening for incoming connections. We will use port `3001`.

    > Hint: For bonus points, you can move the port number into your `.env` file

5. Use `console.log` to test your endpoint. Use an API testing tool such as POSTMAN or Insomnia to test your endpoint.

If your endpoint works, move onto the next assignment.

## Assignment 6 - Mongoose - Complete the "UserSchema" Schema

Edit the file `Schemas/UserSchema.js`.

1. Import `mongoose` into `Schemas/UserSchema.js` using the `require` function

2. Use the following starter code:

```javascript
const UserSchema = new mongoose.Schema({
	firstName: { type: String, required: true }
});

module.exports = UserSchema;
```

3. Complete the Schema above. The UserSchema schema should have the following data:

    ```
    username
    password
    firstName
    lastName
    dateOfBirth
    email
    telephone
    gender
    ```
    
    `dateOfBirth` should be of type `Date`
    
    The rest should be of type `string`

## Assignment 7 - Adding validation to the "UserSchema" schema

All values **except** for `telephone` and gender should be `required`

`gender` should have an `enum` validation. It should accept only the strings `'Male'`, `'Female'` and `'N/A'`.

`gender` should default to the string `'N/A'`

## Assignment 8 - Creating a model from the schema

Now our schema has been defined, we must instantiate it into a model.

By doing this it will give us access to methods such as `save()`, allowing us to quickly and easily save data into our database.

Edit the file `Models/User.js`.

1. Import `mongoose` into `Models/User.js` using the `require` function

2. Import your `UserSchema` into `Models/User.js` using the `require` function

3. Use the following starter code:

```javascript
const User = mongoose.model('User', UserSchema);
```

4. Export your variable `User` from `Models/User.js` using `module.exports`

## Assignment 9 - Saving data to your model

When we save data to our model with the `save()` method, the data is automatically saved to the database.

We will test saving data to our model, by passing in some data.

For now, we will use dummy data. Later we will use real data posted to the API.

1. Import `User` into `server.js` using the `require` function

2. Inside your `POST` `/user/register` route, create an instance of `User`, and save the result into a variable, `newUser`. Pass an object into the `User` constructor. For example:

    ```javascript
    const newUser = new User({
       firstName: 'Franco'
    });
    ```

3. Fill in the rest of the expected fields with "dummy" data (refer to the schema you wrote if you need a reminder). For the `date` you can simply use `new Date()`.

4. Your new variable `"newUser"` is an instance of the model `"User"`. You can think of the model like an ES6 class. It has access to all the mongoose Model methods.

    - Run the `save()` method on `newUser`. `save()` returns a promise.
    - If the promise is successful, post a message in the `console`
    - If the promise is unsuccessful, post the error in the `console` and investigate why

5. Use a MongoDB database viewing tool such as Compass to check the data that you tried to save, is saved correctly in the database

Once your test is successful, move onto the next assignment

Research: [Mongoose Model API (en)](https://mongoosejs.com/docs/api/model.html)

## Assignment 10 - Preparing to receive data from POST

We will replace our dummy data with the data we receive from the POST request

Before we can do this, we must prepare our application to receive data.

1. Install the cors middleware package

```shell script
npm install cors
```

2. Add the middleware `express.json()` and `cors()` to your server application

    > Hint: Don't forget to import `cors` before trying to use it

## Assignment 11 - Receiving data from POST

Inside your `/user/register` endpoint:

1. Use `console.log` to test that you are properly receiving the `body` data from the request. Use POSTMAN or Insomnia to test your API.

Once you are happy with your test, move onto the next assignment.

## Assignment 12 - Connect our POST data to our Mongoose model

1. Take the `body` parameters from the POST request, and map them into the object you created for your `User` constructor - replacing the dummy data you previously used

2. Move your `response.send()` into the promise methods `then` and `catch` (or `try` and `catch` if you are using these instead).
    - It should return a status code of `200` if the promise resolves
    - It should return a status code of `400` if the promise rejects

    > Hint: You should not have `response.send()` outside of these methods, as it will confuse `express` and cause your application to stop working.

Test your application using your API testing tool.

## Assignment 13 (optional) - Build a frontend to make these requests

Build a frontend which will make the `POST` request you were previously testing with your API testing tool.

1. The frontend should consist of a `<form>`, which will take the following details:

    - username
    - password
    - firstName
    - lastName
    - dateOfBirth
    - email
    - telephone
    - gender
    
    There should be a button to `Submit` the form
