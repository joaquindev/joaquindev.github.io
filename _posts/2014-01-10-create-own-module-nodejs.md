---
layout: post
title:  "Create our own module in Node.js"
date:   2014-01-10
categories: nodejs modules exports  
---

We have some functions that we want to be available through the `require` functionality in **node.js**. What does `require` returns? We use `require` when we want to reuse our code all throughtout our node.js application. Imagine we want to have a function which gives us a timestamp, after having a look at the [Javascript Date Object][dateobject] we create the function we want to use from any point in our app in our file `clock.js`: 

```javascript
//clock.js
var whatTimeIsIt = function(){
  console.log(new Date().toDateString());
}
exports = greetings;
```

As you can see, to define what we return when we *require* this function we have to use the `exports` object. Then in our application after *requiring* `clock.js` file, we can use it like this: 

```javascript
//app.js
var whatTimeIsIt = require('./clock');
whatTimeIsIt();
```

Another way is exporting a function into the `exports` object so when we write our require instruction later, we are going to get back and object (instead of a function), so we create export our function into the `exports` object like this:  

```javascript
//clock_object.js
exports.whatTimeIsIt = function(){
  console.log(new Date().toDateString());
}
```

And then, we require our file, we get back an object and then we can call the function we exported in that object: 

```javascript
//app.js
var object = require('./clock_object');
object.whatTimeIsIt();
```

Notice that there is no need to assign what is return by require to an object, we can call require and call the function that it returns directly:

```javascript
console.log(require('./clock_object').whatTimeIsIt();
```

Sources: Inspired by [Real Time Web Code School Course][realtimewebcodeschoolcourse]





<!-- [id_ref]: URL--> 
[dateobject]: http://www.w3schools.com/jsref/jsref_obj_date.asp
[realtimewebcodeschoolcourse]: https://www.codeschool.com/courses/real-time-web-with-nodejs
