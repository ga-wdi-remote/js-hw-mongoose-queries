# Mongoose Vampires


## Exercise Objectives
Previously we were working with hard-coded data, like an array of objects, that lived in our app. In tonight's assignment, we will isolate the "MODELS" part of MVC, and get practicing using MongoDB and Mongoose queries to directly manipulate vampire data in our database!

Since we are not using Express for this assignment, we will be coding our Mongoose queries directly in the `server.js` file. That means after each successful query, you should comment out that code to avoid duplication and clutter in the database and terminal.

### Resources
Utilize the following resources to research the commands you will need
(MongoDB Manual)[https://docs.mongodb.org/manual/reference/operator/query/#query-selectors]

(Mongoose Docs)[http://mongoosejs.com/docs/guide.html]

## Setup
1. In terminal, start your mongo server with `mongod`.

2. Open a new tab (`cmd + t`) and open your mongo console with `mongo`. Make sure there are no errors/ no other instances of this running.

3. Inside your mongo console, connect to a new database called monsters with the command `use monsters`

4. Open a new tab in terminal (3 altogether, what a pro!) and navigate to your homework folder for tonight. You will have:

  - A folder for your `vampires_mongoose`

  - A file for writing your app called `server.js`. Your requirements have been filled out for you. You will write ALL of your database queries in here. Comment out your database commands as you get them working so that you're only running one at a time. This is where we will be looking for your work.

  - A file called `populate_vampires.js` inside `seed` that includes some data that you will use to populate our database (part 2).

  - A file called `models/vampire.js`. This is where you will define your scheme and export it.

5. In the `vampires_mongoose` directory, go ahead and run `npm install mongoose` to install and use mongoose. This is the only library we are dealing with today.

## Directions

### Part 1: Building a Schema

<details><summary>:bulb: RECAP: Schema, Why Schemas, Mongoose</summary>
## What is a schema?

A schema is a way to organize, ahead of time, what a group of data is going to look like. This can be at various levels of a database depending on what kind of databases you are using.

Mongo, is schema-less on the database level. It doesn't care what the data looks like and will take in virtually anything as long as it's syntactically correct.

## Why they are important

Even when you are using mongo, an inherently schema-less database, a schema can be very helpful. It helps control what is going into the database so that you can both know what is going into it, and to make validations.

## Mongoose

This is where mongoose comes in. Instead of making sure everything we are putting into our database makes sense and conforms to some type of structure manually, mongoose allows us to define schemas. Mongoose, in the background, can enforce these schemas (as strictly as you like) in order to make sense of the data going into the database and to allow validation. It provides powerful and simple to use tools to do this.
</details>


Lets design a schema using mongoose and then use it to create some documents and eventually query for those documents.

1. Inside the `models` directory in `vampire.js`, you will create your schema and model in this file. Follow the prompts to guide you.

To start your schema:
```javascript
var vampireSchema = new Schema({
//write your schema fields here

});
```

Our vampire collection will look like this:

``` javascript
var vampire = {
  name: 'Count Chocula',
  hair_color: 'brown',
  eye_color: 'brown',
  dob: new Date(1971, 2, 13, 7, 47),
  loves: ['cereal','marshmallows'],
  location: 'Minneapolis, Minnesota, US',
  gender: 'm',
  victims: 2,
}
```

- Build a vampire **schema** and **model** that matches the object above. Export your model.

<details><summary>.. Stuck?</summary>
- Your Vampire schema should look like:

  ```
  var vampireSchema = mongoose.Schema({
      name: String,
      hair_color: String,
      eye_color: String,
      loves: [String],
      dob: Date,
      location: String,
      gender: String,
      victims: Number,
      title: String
  });
```

- Check your syntax in `models/vampire.js` -- CAPITLIZATION counts here.

- Did you module.exports your model correctly?
</details>

<details><summary>:bulb: Bonus objective: Mongoose Validations</summary>
Go to the Mongoose documentation to learn more about validations and defaults: http://mongoosejs.com/docs/api.html

The name field is required, so make sure that the schema accommodates for that.

Also, no vampire will have less than 0 victims, so add that into the schema as a validation.

Lastly, set the default value of the hair color to blonde.
</details>


- Set up your `server.js` file to connect to your `monsters` database, and follow the prompts. Test your server is connected when you run `node server.js`.

- :dart: Git add, git commit -m "set Vampire model scheme and hooked up the DB"


### Part 2:  Add Vampire Data with Mongoose

Insert into the database using **create** method:

- Using the js objects of vampires in `populate_vampires.js`, add the vampires to a vampires collection.

- You can do this simply by providing this array to the insert method and it will create a document for each object in the array. You have a choice to either
  - paste the entire array into this insert command
  - OR you could just export the array into a variable and insert it with the variable name with the following code:

```javascript
//server.js
//seed vampire data
Vampire.collection.insertMany(vampireData,
  function(err, data) {
    console.log("added provided vampire data")
    mongoose.connection.close();
  });
```

- Test that this was successfully done in two places:
  - Terminal: run `node server.js` and see the console log
  - MongoDB: type `show collections` and confirm `vampire` shows up
- :dart: Git add, git commit -m "inserted vampire seed data"

<details><summary>Oh no! I messed up..</summary>
SOS: If you want to drop your database, in the terminal tab that has mongo open enter: `Vampire.collection.drop()`. Doing this at any time in your mongo database will delete your Vampire collection (no coming back!). To re-populate your database, you will have to uncomment Part 2's code and run the script again.
</details>

:eyes: **NOTE!** notice a few things:
1. We are closing the mongoose connection after this query ends within the callback function. You will be doing the same for all of YOUR queries.
2. We tested to see if our query worked by checking the console (terminal) and the database itself (mongo) using mongo syntax. You will be doing the same for all of YOUR queries.

### Part 3: Create even Newer Vampires

1. Using mongooses's create method, create 4 new vampires with any qualities that you like two should be male and two should be female. Write a console log upon success, "We've created vampires".

Hint, you can do all 4 at once, just mind the syntax and follow your Vampire schema:
```
//server.js

Vampire.create({
    object1 stuff
  }, {
    object2 stuff
  }, {
    object3 stuff
  }, function (err, data){
    console.log("We've created vampires!");
    // This link will close the database connection:
    mongoose.connection.close();
  });
```

2. Save your file and test that this was successfully done in two places:
  - Terminal: run `node server.js` and see the console log
  - MongoDB: type `db.vampire.find()` and confirm your vampires are there
- :dart: After each vampire, Git add, git commit -m "new vampire created"

### Part 4: Querying with `.find()` and `.findOne()`

[Mongoose Docs for .find, .findById ](http://mongoosejs.com/docs/api.html#model_Model.find)
[Mongoose Docs for .findOne()](http://mongoosejs.com/docs/api.html#model_Model.findOne)
[MongoDB Query Selectors & Operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors)

Write a different mongoose query for each of the following using `Vampire.find()` and console log the data. You can test each query by running `node server.js` and seeing the data print to terminal:

1. For example, go ahead and test out the code for "Find all the vampires that that are females". Make sure your quieries also console.log the data and close the mongoose connection:

```
 Vampire.find(
   { gender : 'f' },
     function ( err, data ){
       console.log( data );
       mongoose.connection.close();
   });
```
1. Find one vampire named Armand
2. Find all vampires that have greater than 500 victims
3. Find all vampires that have fewer than or equal to 150 victims
4. Find all vampires that have a victim count is not equal to 210234
6. Find all vampires that have a title property
7. Find all vampires that do not have a victims property
8. Find all vampires that love brooding or being tragic


### Part 5: Update with `.update()` or `.findOneAndUpdate()`

- [Mongoose Docs on .findOneAndUpdate  ](http://mongoosejs.com/docs/api.html#model_Model.findOneAndUpdate)
- [Moongoose Docs on .update()](http://mongoosejs.com/docs/api.html#model_Model.update)
- [Mongoose Docs on .findByIdAndUpdate](http://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate) (we're not using IDs here.. so..)
- [MongoDB Query Selectors & Operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors)

Write a different mongoose query for each of the following using `Vampire.updateOne()` and console log the data. You can test each query by running `node server.js` and seeing the data print to terminal:

1. Update 'Dracula' to have a gender of 'f'
3. Update 'Dracula' to have an array called 'hates' that includes 'clothes' and 'jobs'
4. Update 'Lestat's' hates array also to include 'alarm clocks' and 'jackalopes'
5. Rename 'Eve's' name field to 'Steve'

### Part 6: Remove with `.remove()`

[Mongoose Docs on .remove()](http://mongoosejs.com/docs/api.html#model_Model.remove)
[MongoDB Query Selectors & Operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors)

1. Remove 'Claudia'
2. Remove a single document wherein the hair_color is 'brown'
3. We found out that the vampires with the blue eyes were just fakes! Let's remove all the vampires who have blue eyes from our database.
<hr>

### When you're done

When you're done with the exercise, don't forget to properly shut down mongo and mongod. The terminal shortcut is `Ctrl + C`.

### Reach Goals

#### Select objects that match one of several values
Select all the vampires that:

1. love either frilly shirtsleeves or frilly collars
2. love brooding
3. love at least one of the following: appearing innocent, trickery, lurking in rotting mansions, R&B music
4. love fancy cloaks but not if they also love either top hats or virgin blood * Hint-You will also have to use $nin *

#### Negative Selection
Select all vampires that:

1. love ribbons but do not have blonde hair
2. are not from Rome
3. do not love any of the following: [fancy cloaks, frilly 4. shirtsleeves, appearing innocent, being tragic, brooding]
5. have not killed more than 200 people

#### Replace the following:

1. replace the vampire called 'Claudia' with a vampire called 'Eve'. 'Eve' will have a key called 'portrayed_by' with the value 'Tilda Swinton'
2. replace the first male vampire with another whose name is 'Guy Man', and who has a key 'is_actually' with the value 'were-lizard'

#### Make an index route
1. Make an index route that will res.send the json for those all of the items in our database.

## Submitting Your Work

  When you're ready to submit your work,

  1.  Add, commit, and push your code to your fork of the class repo.
  2.  File an issue on the class repo titled "Your Name -- wXXdXX".

  The issue should include:

  -   A link that points back to your fork.

  -   A 'comfort' score on how you feel about the material, from 1 (very
      uncomfortable) to 5 (very comfortable)
