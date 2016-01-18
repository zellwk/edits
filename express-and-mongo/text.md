# Building a Simple CRUD Application with Express and MongoDB

I thought I'd docment the basics for someone who wants to learn to build their first application with Node. 

## CRUD, Express and MongoDB

CRUD, Express and MongoDB. All these terms are big words for one who has never touched any server-side programming in their lives. Let's quickly introduce what they are before we diving into the tutorial. 

[Express]() is a framework for building web applications on top of Node.js. It simplifies the server creation process that is already available in Node. In case you were wondering, Node allows you to use JavaScript as your server-side language.

[MongoDB]() is a database. Databases are where information related to your web application is stored. 

[CRUD]() is an acronym for Create, Read, Update and Delete. CRUD is a set of operations we can get servers to execute. The following would sound more reader-friendly:

- Create - Make something
- Read - Get something
- Update - Change something 
- Delete - Remove something

If we put CRUD, Express and MongoDB together into a single diagram, this is what it would look like: 

<figure>
  ![Alt text](/path/to/img.jpg "Optional title")
</figure>

Does CRUD, Express and MongoDB makes more sense to you now? 

Great. Let's move on.

## Getting Started 

We're going to build a simple list application that allows you to keep track of things within a list (like a Todo List for example). 

This is where you can find the [finished code]() and a [live demo]().

Note: In case you were wondering, this isn't MVC yet. We're mainly focusing on how to use CRUD, Express and Mongo DB in this tutorial. MVC relies more on client-side JavaScript. 

You'll need two things to get started with this tutorial: 

1. You aren't afraid of typing commands into a shell. Check out [this article]() if you have no idea what I'm talking about.
2. You need to have [Node]() installed. 

To check if you have Node installed, open up your command line and run the following code:

```bash
$ node -v
```

You should get a version number if you have Node installed. If you don't, you can install Node either by downloading the installer from Node's website or downloading it through package managers like [Homebrew]()(Mac) and [Chocolatey]()(Windows).

## Getting Started 

Create a folder called `list-app` anywhere on your computer and navigate to it. Then, run the `npm init` command. 

This command creates a `package.json` file that helps you manage dependencies that we will install into the application in a short while. 

```bash
$ npm init
```

Just hit enter through everything that appears. I'll talk about the ones you need to know as we go through the tutorial.

## Running Node for the first time in your life

Since Node allows us to write JavaScript on the server-side, it also means we can execute JavaScript files without the use of browsers. 

The simplest way to use node is to run the `node` command with a path to a file. Let's create a file called `server.js` to run node with. 

```bash
$ touch server.js
```

When the execute the `server.js` file, we want to make sure it's running properly. To do so, simply write a `console.log` statement in `server.js`: 

```javascript
console.log('May the Node be with you')
```

Now, run `node server.js` in your command line and you should see the statement you logged: 

<figure>
  ![]()
</figure>

Great. Let's move on and learn how to use Express now.

## Using Express 

We first have to install Express before we can use it in our application. Installation is easy, we can use the Node package manager (npm) which comes bundled with Node to install Express. 

Run the `npm install express --save` command in your command line: 

```bash
$ npm install express --save
```

Once you're done, you should see that npm has saved Express as a dependency in `package.json`. 

<figure>
  ![]()
</figure>

Next, we use express in `server.js` by requiring it. This is also how you can use other packages you've installed with npm.

```javascript
const express = require('express');
const app = express();
```

One of the main things express helps us with is to create a server that browsers can connect to. We can use the `.listen` method that express provides to do so: 

```javascript
app.listen(3000, function() {
  
})
```


Note: This, of course, isn't the best example. We could create single page apps with JavaScript so we don't force users to refresh their browsers whenever there is a need to perform CRUD operations. 

It however, forms a good base for those who are starting out with the backend side of things. 