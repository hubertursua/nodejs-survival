# Node.js for Survival
This tutorial covers essential node.js concepts to help you get started and survive your first node.js project. Whether you plan creating a website, an API, or a background task using node.js, this tutorial will cover the basic things that you need (and more often left out by other tutorials).

This tutorial will cover the basics of the following:

* how to use the Node Package Manager
* how to code and understand asynchronous commands
* how to use and create node modules

I won't teach you how to create websites or APIs nor will I teach you JavaScript 101.

After this tutorial, you can jump to other tutorials that are specific to what you want to create using node.js.

## Assumptions

Before you dive in this tutorial, I assume you know the following:

* Any object-oriented language (as I have OOP analogies)
* Basic JavaScript
* JSON (or at least you know what it looks like)
* How to use your operating system's terminal (command line)

## The good, the bad, the node

### The bad sides of node.js
Let me start of by convincing you that you should not use node.js.

* Single thread, usually per application
	* You need to setup a script to start/stop it properly
	* If a sub-process in the queue crashed, your whole app dies
* Asynchronous
	* Confusing if you're a procedural programmer
	* Coding can get messy if you don't know what you're doing

### The good sides of node.js
* Modular approach (via npm, node API, or write your own)
* Single thread
	* Doesn't eat too much RAM
	* It manages its own queue; and it does it extremely well
	* Makes it easy to scale
* Asynchronous, because of the queue thing and because of JS

### Other benefits of node.js

Aside from those mentioned in the previous section...

* Unified language from UI (web JS) to backend (node.js)
* Any proper web developer knows JavaScript

I recommend you read [Tomislav Capan's article](http://www.toptal.com/nodejs/why-the-hell-would-i-use-node-js) for a more comprehensive discussion on why you should or should not use node.js.

## Benchmarks

I won't comment. Just see the benchmark for yourselves. Some benchmarks have flaws in how they conduct the comparisons since they compare difference software platforms.

* [http://code.google.com/...](http://code.google.com/p/node-js-vs-apache-php-benchmark/wiki/Tests)
* [http://net.tutsplus.com/...](http://net.tutsplus.com/tutorials/javascript-ajax/node-js-for-beginners/) \*
* [http://blog.loadimpact.com/...](http://blog.loadimpact.com/2013/02/01/node-js-vs-php-using-load-impact-to-visualize-node-js-efficency/)

\* Just check the benchmarks. The tutorial itself has some incorrect parts (e.g. the installation).

## Difference with web JavaScript

**Syntactically, [virtually] none** -- Node.js is a software platform that uses JavaScript. It is packaged with the V8 JavaScript Engine, which is the soul of Google Chrome web browser. Strictly, there could be some difference because of the ECMAScript versions but those differences will just be in the 0.01% of the code that you write. That being said, if you know basic JavaScript and you know it well, you will survive node.js.

**Browser APIs are not available (e.g. `document`, `alert()`)** -- Well, node.js is not run for a web page because it wasn't designed for that. You can do a lot more other than DOM manipulation. It's as good as any platform out there, ranging from creating websites and APIs to low level tasks.

**JS scripts that use browser APIs will also not work** -- Pretty much the same reason as above. This means, you can't use jQuery in node.js; and if you use node.js for a while, you'll realize you don't need it.

**Most JS scripts are not really meant for node.js use** -- To make a JS script to work, you have to make sure it's written in pure JavaScript (no JavaScript browser APIs). Then you have to convert it into a node.js module. Sounds hard, right? If you have a favorite script that you use for the web, let it go. Search node.js' package manager (npm) for an equivalent. It's a better option and they are designed for node.js use.

## Installation

Go to [nodejs.org](http://nodejs.org) and download the installer

If you want to install via a package manager, see [this page](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager).

When you're done, check by running `node` and `npm` in the terminal.

```bash
$ node
> 1 + 1
2
>

# Press Ctrl+C twice to exit
```

When you type `npm`, you should see a list of commands available.

## Hello world

Let's start off with a traditional hello world example. Create a JavaScript file named `hello.js` and type the code shown below.

```javascript
for(var i = 0; i < 10; ++i) {
	console.log('hello world ' + i);
}
```

Now, go back to the terminal and type:

```bash
# Make sure you're in the directory of your file
$ node hello
```

Notice that we did not have to type the `.js` extension; but it will also work if you did.

We can't use `alert()` since this isn't a browser. Instead, we'll use `console.log()` to display our output in the terminal.

## Node Package Manager (npm)
One of the wonderful things about node.js is `npm`. A nice feature of `npm` is it allows you to install published node.js packages for your project. It also lets you create and manage a file named `package.json` which is very useful for handling dependencies and package versions.

For a summary, here are the basic `npm` commands that you need to learn:

* `npm init`
* `npm install <package_name>`
* `npm install --save <package_name>`
* `npm install --save-dev <package_name>`
* `npm install`
* `npm install -g <package_name>`

### `npm init`

This let's you create a simple `package.json`. It will ask you a few questions, then generate the file.

If you want to learn more about package.json, [nodejitsu.com has this awesome interactive cheatsheet](http://package.json.nodejitsu.com). You can also view the [official documentation](https://npmjs.org/doc/json.html)

### `npm install <package_name>`

This will install a package from the npm repository.

Often when you find a node.js package, you'll see in the installation instructions something like `npm install <package_name>`. Just type that in the root of your project to install the package.

Let's try it out using the [request](https://github.com/mikeal/request) package.

```bash
$ npm install request

# NOTE: A lot of URLs will print out in your terminal. I'll discuss this later.
# WARNING: You'll encounter an error if you don't have permissions to write in the current directory.
```

If things went well, you'll notice that you now have a `node_modules` directory. Inside that will be the request package. All of the packages that you install will be inside this directory.

#### Dependencies
If you open the `node_modules/request` directory, it also has its own `node_modules` directory containing other packages. These are the dependencies of the `request` package. `npm` installed them for you as you saw awhile ago in the terminal with the flood of URLs printed out. `npm` and `package.json` will handle all of the dependencies all packages related to your project.

To save all the dependencies of your project, you have to record them in `package.json`. The `--save` and `--save-dev` will take care of those. Read the next sections for more details.

#### Ignore `node_modules`
If you're working with a versioning control like Git, do not commit `node_modules`. Some of the packages are compiled specific to your machine.

In Git, you need to create a file named `.gitignore` in the root of your project and add `node_modules`. If you already have that file, open it and add `node_modules` in a new line.

You should not modify the packages inside `node_modules`. If you want to modify the behavior of a package in some way, fork the Git project of the package, modify it, and add install the modified forked project. You can install a package using the command `npm install <git_repository>`.

### `npm install --save <package_name>`

To record a package as a dependency of your project, type in the terminal:

```bash
$ npm install --save request
```

Your `package.json` will now have a `dependencies` property containing the `request` package and its version number. The `~` symbol before the version number means *any minor version*. So for example, the version is `~2.3.14`. Your package will upgrade to version `2.3.16` or `2.4.0` but not `3.0.0`.

All of the packages that you use in your project should be installed using this method as it will keep your teammates sane.

### `npm install --save-dev <package_name>`

This is pretty much the same as `--save` except that you're specially marking the package as a development dependency. This usually include packages used for compiling, minifying, and testing. Those packages will not be install in the production environment.

```bash
$ npm install --save-dev grunt
```

If you check `package.json`, you'll see a `devDependencies` property.

### `npm install`

Now that you marked all the dependencies of your project in `package.json`, try to delete the `node_modules` directory and let's assume that we freshly got the project from Git. Type the command:

```bash
$ npm install
```

This will install all the packages found in your `package.json`. If you're working in a team, this will make sure that all of you have the correct package dependencies and even the correct version number of each package.

#### Upgrading packages
In case you want to upgrade your packages, just run again `npm install` and it will upgrade all your packages. It will follow the version rules stated in the `dependencies` and `devDependencies` properties of your `package.json`.

### `npm install -g <package_name>`
The last command that you need to survive is this. The `-g` means *global*. It will install the package in your machine, not just for your project.

Usually, these are the packages that have terminal commands. And more often, these are the packages that you also add in the development dependencies.

Let's create a new project and go to that folder via the terminal. No need to run `npm init` for now. Let's install [Express](http://expressjs.com) which is a web app framework (if you're interested in making web pages and APIs using node.js, this is a good start).

```bash
$ npm install -g express

# WARNING: If you encounter an error, make sure you have write permissions. For UNIX systems, run as sudo. For Windows, you might need to run the command prompt as administrator.
```

When you install Express as a global package, it has a nifty command which generates a basic scaffolding for your project. Try running this command inside your empty project directory:

```bash
$ express
```

It will generate some files, including a package.json, for your project. You just have to run `npm install` and you're ready to go.


## Asynchronous pattern

Coding in node.js is really easy. You're spoiled by the awesome packages that you can just quickly install and use. The real hard part is writing proper code in the node.js way. And this is done by maximizing the use of asynchronous calls.

Asynchronous are non-blocking tasks. This means that once it's called, the interpreter goes to the next task, allowing you to virtually multi-task.

Asynchronous also spells *agony* for developers who have procedural programming backgrounds. This is because, the commands do not finish sequentially. Your commands follow a mantra *"When I'm done, I'm done. Don't bug me."*

Take note that not all node.js commands are asynchronous, but most are. If you can write a piece of code asynchronously, do that instead of fighting with the other commands that follow the async pattern.

Let's go back to our previous example (the one with the request package). Create a new JavaScript command called `race.js` and type the following code:

```javascript
var request = require('request'); // I'll discuss the require() command later

request('http://www.google.com', function (err, res, body) {
	if (!err) {
		console.log('Got google');
	}
});

console.log('Reached the end');
```

Run the script in your terminal:

```bash
$ node race
```

Notice that *"Reached the end"* printed out first. This is because the `request` command took some time to process. node.js didn't wait for it to finish since it's asynchronous anyways. It printed out *"Got google"* after it was done.

Take note that there are cases when the asynchronous call will be fast enough that it'll seem that the commands are sequential. But do not bet on this.

### Callbacks

An easy way to detect if a command is asynchronous is to check the parameters that it accepts. Often, it will accept a function. And more often, it's the last parameter. This is what you call a **callback function**. This function is triggered when the command is done.

In the previous example, you see that `request()` had two paramters. The first is a URL. The second is a function--a callback function.

```javascript
function (err, res, body) {
	if (!err) {
		console.log('Got google');
	}
}
```

NOTE: Some functions have more than one callback function, which are triggered at different events of the command (e.g. initialization, processing, done, error). But in most cases, it's just one.

### Error handling

Most node.js functions do not throw an error. They return it instead. This has something to do with the asynchronous nature and node.js being a single thread process. Returning the error is safer than throwing it and risking the process to end because of a missing `try-catch`.

Let's check out the callback of `request`:

```javascript
function (err, res, body) {
	if (!err) {
		console.log('Got google');
	}
}
```

The first parameter of the callback is an error variable. This is why we have an `if` statement checking if there's an error.

In most cases, the error variable will be either `undefined` or `null` when there is no error thus the `(!err)` condition will evaluate as `true`.

In case there's an error, the error variable will be an instance of the `Error` class of JavaScript and the `(!err)` condition will evaluate as `false`.

TIP: The norm is that the first variable of a callback function is the error variable. But check the documentation of the command just to be sure.

EXTRA NOTE: Stricty, JavaScript doesn't have classes. There are things that look like classes called prototypes.


## Using packages via `require()`

The `require()` command should be familiar since we used this in the previous example. Whenever you want to use a node.js module, you call this command. `require()` will try to find the node.js module and return it.

In the previous example, we had:

```javascript
var request = require('request');
```

The common way is to name your variable same with the module's name.

Most of your node.js files will be written as node.js modules (how to create a module is in the next section). Therefore, you'll also use `require()` when importing other files. Think of `require()` as an equivalent to `include` or `import` in other languages.

There are different ways to use require. Here are the common cases:

```javascript
// Anything that's installed via npm or is included in the node.js API
var fs = require('fs');

// Anything in the same directory as the current file will need a ./
var foo = require('./foo');

// Anything relative to the current file will also need a ./
var llama = require('./../flying/llama');
var beagle = require('./dogs/beagle');
```

## Node.js modules

With the exception of your main file, most of your JavaScript files will be written as node.js modules. This is analogous to how in most object-oriented platforms you write classes. In node.js, you write modules.

There are 3 ways to write a node.js module. Most often, you'd want to write your files using Pattern #1.

Pattern #1 is analogous to how you write static functions in other OOP languages. Pattern #2 and #3 are similar and they are analogous to how you write non-static functions.

### Node module pattern #1

Let's create a file called `greet.js` containing the following code:

```javascript
exports.hello = function (name) {
	var greeting = 'hello ' + name;

	return greeting;
};
```

In an OOP language, this is similar to you creating a `greet` class with a method `hello(name)`. The `exports` object exposes your `hello()` function, similar to the `public` access modifier in other languages.

Let's create a new file called `app.js`. This will be our main JavaScript file. Let's place the following code:

```javascript
var greet = require('./greet'); // Notice that there's no .js extension

var h = greet.hello("John");
console.log(h);
```

Run `app.js` in the terminal and you should see it work.

```bash
$ node app
```

#### Asynchronous module

Let's modify our code to make it look more like a node.js script by following an asychronous pattern.

```javascript
// greet.js
exports.hello = function (name, callback) {
	var greeting = 'hello ' + name;

	callback(greeting);
};
```

We added a second parameter named `callback`. This is a function which will accept the output of `hello()` as parameters. In this case, it's the contents of `greeting`.

Instead of returning a value, we pass the output by invoking the callback function. This is cool since we can pass multiple values as multiple parameters of the callback function.

We also need to modify our `app.js` to follow the new asynchronous pattern.

```javascript
// app.js
var greet = require('./greet');

greet.hello("John", function (h) {
	console.log(h);
});
```

And run it again:

```bash
$ node app
```

Now, it looks a lot like our `request()` example.

Let's try something while we're discussing again asynchronous calls. Add a console.log() after `greet.hello()`.

```javascript
// app.js
var greet = require('./greet');

greet.hello("John", function (h) {
	console.log(h);
});

console.log('I should be first');
```

If you try to run this, there's a good chance that *"hello John"* will print first. With asynchronous commands, there's no guarantee that it will follow a certain sequence.

### Node module pattern #2
This is the part where it gets a bit confusing but I'll explain all the changes in our module.

```javascript
// greet.js
module.exports = function () {
	var mod = {};

	mod.hello = function (name, callback) {
		var greeting = 'hello ' + name;

		callback(greeting);
	};

	return mod;
};
```

In module pattern #2, to sum it up, we created a JSON object which has a `hello()` function and returned it.

1. `var mod = {};` creates the JSON object.
1. `mod.hello = function (name, callback) { ... }` adds the `hello()` function to our JSON object.
1. Then we return our JSON object.

If you try to run your `app.js`, it will give you an error. We have to change our `app.js` a bit.

```javascript
// app.js
var greet = require('./greet');

var g = new greet();

g.hello("John", function (h) {
	console.log(h);
});
```

We had to create an instance of `greet` before we could call it.

### Node module pattern #3
Pattern #3 uses the `this` keyword.

```javascript
// greet.js
module.exports = function () {
	this.hello = function (name, callback) {
		var greeting = 'hello ' + name;

		callback(greeting);
	};

	return this;
};
```

It's very similar to Pattern #2 except that instead of creating a new object and returning it, we put everything in the current function. The downside of this is you will sometimes encounter referencing issues because of the context of the `this` keyword.

## That's it

Wait, what!?

Yup, that's it. That's all you need to know if you want to survive node.js.

Yes, we haven't gone into writing APIs or websites. That's because it depends on what you want to do. Aside from the basics that I taught you here, node.js development revolves around mastering the use of certain packages. If you want to build APIs, you have to use a certain collection of packages. If you want to build websites, you have to use another set of packages.

My final note for you is you need to master the asynchronous pattern. It's really tricky. Keep things modular. If you have at least 5 nested callbacks, chances are you're doing things the wrong way.

### Recommended packages
I have some favorite packages depending on what I build. There might be better sets but these are what I use to get you started.

#### API development
* `express` -- Web development framework
* `express-path` -- Route mapping for Express (I wrote this so, yeah, I might be biased; but do check it out)
* `mongoose` -- MongoDB driver (very good driver if you want to use MongoDB and documentation is complete but hard to sift through)
* `nodemon` -- Restarts your node process automatically after you save. Excellent pair with Express.
* `request` -- (Optional) HTTP requests for interfacing with other APIs

#### Server-processed web pages
* `express`
* `express-path`
* `jade` -- Alternative HTML syntax (templating, partials, etc.). Express uses this by default.
* `stylus` -- Preprocessed CSS
* `mongoose`
* `nodemon`

#### Static web pages
* `jade`
* `stylus`
* `grunt` -- Useful for creating tasks
* `grunt-contrib-connect` -- Quick and dirty HTTP server for testing your HTML
* `grunt-contrib-livereload` -- Refreshes your browser after it detects changes in your code
* `grunt-contrib-jade` -- Grunt task for compiling Jade files to HTML
* `grunt-contrib-stylus` -- Grunt task for compiling Stylus files to CSS
* `grunt-contrib-watch` -- Observes your files and calls other grunt tasks if something changes

#### Other packages that you should check out

* `forever` -- Keeps a node.js module running in the background
* `goosestrap` -- mongoose models loader
* `grunt-contrib-jshint` -- jsHint forces you to follow code convention rules. This is a grunt task for jsHint.
* `grunt-contrib-uglify` -- Uglify concatenates and minifies JavaScript (those .min.js files). This is a grunt task for jsHint

There are a lot more but I believe these are enough to jumpstart you.

God speed and thank you for supporting node.js.
