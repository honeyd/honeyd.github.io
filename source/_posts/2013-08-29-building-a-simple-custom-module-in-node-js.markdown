---
layout: post
title: "Building a Simple Custom Module in Node.js"
date: 2013-08-29 11:45
comments: true
categories: [node,javascript,development] 
---

Writing modular code is one of my very favorite things to do, so it wasn’t long in my adventure with Node.js before I was trying to understand how to build a custom module. I think I’m finally beginning to grasp exactly how custom modules are typically constructed as well as how they are pulled in as dependencies to my Node application.

First, I’ll create an app.js file from which my experiments will run. For now, I’ll leave app.js empty. Side note: app.js is the typical name of the main file of your new Node.js application.

In the same directory as app.js, I’ll create a new JavaScript file called utils.js. This will be my new module. I’ll add two functions to my utils module that I will be using in my Node.js application.

``` javascript
var addHello = function(name) {
  return "Hello " + name;
};
var addGoodbye = function(name) {
  return "Goodbye " + name;
};
```

Even though my utils module defines two perfectly good functions, these functions are not yet available to my application. In order to bring my utils module into my application, I define a dependency by using the require() function. In my app.js, I’ll add the following:

``` javascript
var utils = require('./utils');
```

Unfortunately, if I try to run `console.log(utils.addHello("Honey"));` in my app.js, the code will break. The `require()` function doesn’t just include the code in my module. Instead, it wraps the code in my module up in a closure that then returns anything set to `module.exports`. In other words, I need to add the functions I created to a special object called `module.exports` so that requiring my module will return the `addHello()` and `addGoodbye()` methods. I can do this my modifying utils.js in a couple of different ways:

``` javascript
var addHello = function(name) {
  return "Hello " + name;
};
var addGoodbye = function(name) {
  return "Goodbye " + name;
};
module.exports.addHello = addHello;
module.exports.addGoodbye = addGoodbye;
```

OR through direct assignment:

``` javascript
module.exports.addHello = function(name) {
  return "Hello " + name;
};
module.exports.addGoodbye = function(name) {
  return "Goodbye " + name;
};
```

Now when I try to use these functions in app.js, everything should work swimmingly.

``` javascript
var utils = require('./utils');
console.log(utils.addHello("Honey"));
//=> Hello Honey
```

So far so good? Good. There are some other things to be aware of.

module.exports vs. exports

You may notice that sometimes, people will use the variable exports instead of module.exports. In my utils.js, I can replace all my references to module.exports with just exports, and everything will continue to work as expected.

``` javascript
exports.addHello = function(name) {
  return "Hello " + name;
};
exports.addGoodbye = function(name) {
  return "Goodbye " + name;
};
```

Remember that wrapper I mentioned, that is responsible for packaging the code in my module? Well, before it pulls in my code, it assigns `module.exports` to `exports`, so that `exports` is just a pointer to `module.exports`. Take a look at this excellent answer from StackOverflow: [Difference between “module.exports” and “exports” in the CommonJS Module System](http://stackoverflow.com/questions/16383795/difference-between-module-exports-and-exports-in-the-commonjs-module-system/16383925#16383925).

In other words, `exports` is a shortcut to `module.exports` (less typing), but it’s not the variable that is returned, so I have to be careful. As a result of this, I see the following pretty often, especially when a custom module is a constructor:

``` javascript
var Book = exports = module.exports = function() {};
```

That’s a lot of assignment! This statement not only ensures that `exports` and `module.exports` are the same, but it also assigns the module function to a variable that can be used for anything I like outside of the constructor function (like adding methods to the prototype, for example).

Interestingly, I can `require()` a directory, not just a single file. Node.js will gladly look in the directory and see if I have an index.js file to use, and pull that in if it exists. If I wanted to package up a bunch of modules into one `require()`, I might add something like this to index.js (assuming one.js and two.js exist in the same folder):

``` javascript
module.exports = {
  one: require('./one'),
  two: require('./two')
};
```

Then I can require the directory in app.js (assuming index.js, one.js, and two.js live in a directory called “stuff”):

``` javascript
var stuff = require(./stuff);
```