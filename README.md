# Backend Notes

## Node
Node is a a framework that allows us to use JavaScript to write server-side logic.

### Installing Node
I used Homebrew to get Node installed. I updated Homebrew with `brew update` and installed Node with `brew install node`.
### Creating a New Node App 
I created a new directory with `mkdir my-first-node-proj`, `cd`'d into the directory and initialized Node with the command `npm init`. _You can bypass the init steps and use the default JSON values by using the `-y` flag after `npm init`._
I used the default `main` value from `package.json` and used `touch index.js` as the entry point to the application. To run the program, from the project directory in terminal I ran `node index.js`.\
In brief, to create a new Node app:
- `npm init` Initialize Node in cwd
- `touch index.js` Create entry point
- `node index.js` Run file in Node

### Node Modules
#### Create a Module
Node modules allow code written in one file to be imported into other files. The first step in creating a module is to `touch` a module file, lets say `myModule.js`. in our module file we can use `module.exports` to store the code that is to be exported. 
```js
// myModule.js
module.exports.myFoods = ['uncle howies pizza', 'mashed potatoes', 'calamari', 'spinach', 'spam and eggs'];
```
Once we've done this, we can access the myFoods array by importing `myModule` into `index.js`.
```js
// index.js
const myModule = require('./module.js');
console.log(myModule.myFoods);
```
#### Import Someone Else's Module
NPM or Node Package Manager gives us access to countless modules that other people have written. To import a module from npm, we first must install it in our cwd. I'll use the awesome module Chalk as an example here. We would install chalk by running `npm i chalk`. Once the module is installed, we import it in our `index.js` file as we did above: 
```js
const chalk = require('chalk')
```
### Gitignore
#### WARNING
Importing a module will create a directory within the cwd called `node_modules`. A very large quantity of data is stored in this directory and it is unnecessary to commit to our repository. Before committing any node project to git, it is vital that we add a `.gitignore` and add the `node_modules` directory to it. To add a gitignore, in terminal we simply type `touch .gitignore` in the project's cwd. Open up the new gitignore file in our code editor, write `node_modules/`, and save the file. Now the `node_modules` directory will not be included in our commits and pushes.

In Brief, to import our own module: 
- `touch myModule.js` Create a module file in cwd
- `const myModule = require('./myModule.js')` _Use require()_ to import module into _index.js_

To import someone else's module: 
- `npm i someModule` Install the module in cwd
- `const someModule = require('someModule')` Use _require()_ to import module into _index.js_

### Express
Express is a web application framework in Node.js. To install Express, first initialize node in your cwd with `npm init`. Then install express by typing `npm i express`. In your entry point file (the index.js created when initializing node) you import Express by invoking `require()` and then create an instance of an Express app by invoking `express`: 
```js
const express = require('express');
const app = express();
```
Next thing to do is to create a Home Route. A route in this case means the specific URL location of certain data. For example: _www.mypage.com/about_ the route here would be `/about`. Routes are set up like so: 
```js
const express = require('express');
const app = express();

// req = request, res = response
app.get('/', function(req, res) {
    res.send('Hello, World!');
});
app.listen(8000);
```
At the end of the above snippet, we invoke the `listen()` method to serve our webpage to localhost at port 8000.
#### Request
The request object is frequently abbreviated to `req`. It contains all the necessary data about the incoming request. What the user was requesting, their browser, etc. The three most commonly used keys are:
- `req.body` Where submitted form data is stored
- `req.params` Where special route variables are stored
- `req.query` Where the query string data is stored
#### Response
The response object is frequently abbreviated to `res`. There are several methods we can invoke on `res`:
- `res.send()` Like a console.log() for network requests. Good for testing.
- `res.sendFile()` Can send an entire static file back
- `res.render()` Used to render data into templates with selected template engine. 
- `res.json()` Used to send object data back as JSON. 

### Views
Views in this context is an HTML document that contains a template and produces a response for the browser. The View collects input data from various data sources, finds a template, and combines the data and the template for HTML output. So to implement Views, first we eed to create a `views` folder inside our project directory. Within the `views` directory, we need to create some HTML files and add some basic HTML to them. Then, in our `index.js` file we would would write: 
```js
app.get('/', (req, res)=> {
    res.sendFile(__dirname+'/views/index.html');
})
```
The `__dirname` Node keyword is used to easily get the absolute path of the view you wish to serve (relative file paths wont work with the `sendFile()` method!)

### Templates
So far this readme has only gone into sending HTML files. Now to customize what is on those pages. DOM manipulation is only front-end! This is back end! This is where *Template engines* come in. Template engines allow us to inject values into our HTML files along with Javascript logic. 

#### EJS: Embedded Javascript
EJS is one of the most popular Javascript template engines. It's an npm package, so we install it in our project directory with `npm i ejs`. Now we set the view engine to EJS. The view engine helps us take data from the backend and render it in our final HTML. To set the view engine, above our routes we would use
```js
app.set('view engine', 'ejs');
```

At this point we will need to rename our .html files to .ejs files and replace calls to `res.sendFile()` to `res.render()` like so:
```js
app.get('/', function(req, res) {
    res.render('index');
});
```
As long as our .ejs files are nested within our views folder, we can pass the filename sans extension to `res.render()`.
#### Templating with Variables! 
We are able to pass an object as a second parameter to `res.render()` and then access the values stored there in our ejs template. 
```js
// index.js
app.get('/', (req, res)=> {
  res.render('index', {name: "Barent", age: 30});
});
```
To output those values in our ejs template we use `<%= %>` squid syntax. For example:
```js
// index.ejs
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
    <h2>You are <%= age*7 %> in dog years.</h2>
  </body>
</html>
```
To run a line of js in an ejs template without displaying it (logic for instance), we use `<% %>` ice-cream cone syntax. The javascript used in ejs can include variable declarations and iterators as well: 
```js
// index.ejs
<body>
    <h1>Hello, <%= name %>!</h1>
    <% var dogAge = age*7 %>
    <h2>You are <%= dogAge %> in dog years.</h2>
    <% var status %>
    <%if (dogAge<100) {%>
        <% status = 'young' %>
    <%} else {%>
        <% status = 'old' %>
    <% } %>
    <h3>This means you are <%=status%>!</h3>
</body>
```
#### Partials (no dentures)
Partials are used to DRY up code. Ecumenically used headers and footers are often moved into their own separate views (called partials) and are then rendered into each page rather than being written over and over. Create a new directory called `partials` and touch the files you need within that directory. A sample partial:
```js
// partials/header.ejs
<header>
    <img src="http://placekitten.com/500/500">
  </header>
```
We then include our partial in our `index.ejs` file:
```js
<body>
    <%- include('../partials/header.ejs') %>
    <h1>Hello, <%= name %>!</h1>
</body>
```
### Layouts & Controllers
#### EJS Layouts
While partials DRY up the code a little bit, EJS Layouts take interchangeability to the next level and make a big difference with larger apps. EJS Layouts is a Node package (surprise!) that allows us to create a boilerplate (layout) that we can inject content into based on which route is reached (the _about_ page gets different info than the _login_ page for example.) Layouts generally include header/footer content you want to display on every page of your application. To get started with EJS Layouts– you guessed it, install it with npm:
```js
npm i express-ejs-layouts
```
Then go to `index.js`, require the module, and add it to the app: 
```js
const express = require('express');
const app = express();
const ejsLayouts = require('express-ejs-layouts');

app.set('view engine', 'ejs');
app.use(ejsLayouts);

app.listen(8000);
```
##### app.use() and Middleware
`app.use()` is an express method that indicates Middleware. Middleware functions intercept the request object as it comes in from the client, prior to that request hitting an endpoint. Think of it like the bouncer who stands between you and the entrance of the bar. 
#### EJS Layouts (cont.)
Once we set up `express-ejs-layouts`, we can create a layout in the root of our `views` folder. This layout *MUST* be titled `layout.ejs`. It is required by `express-ejs-layouts`. 
```js
// views/layout.ejs
<!DOCTYPE html>
<html>
    <head>
    <title>Some Webpage</title>
    </head>
    <body>
        <%- body %>
    </body>
</html>
```
`layout.ejs` will be used by all pages and the content will be filled in where the `<%- body %>` tag is placed. `<%- body %>` is a unique tag used by `express-ejs-layouts` and cannot be renamed. Take your creativity elsewhere! 
After `layout.ejs` is created in the views directory, we can create our app .ejs pages and create their routes inside `index.js`.
```js
// index.js
app.get('/animals', (req, res)=> { // somesite.com/animals
    // render animals.ejs along with a js object
    res.render('animals', {title: 'Favorite Animals', 
    animals: ['sand crab', 'corny joke dog']});
});
```
Once we do this, we can access 
```js
// animals.ejs
<h1><%= title %></h1>
<ul>
    <% animals.forEach((animal)=>{ %>
        <li><%= animal %></li>
    <% }) %>
</ul>
```
`layout.ejs` will take all of the info in `animals.ejs` and slap it into the body of an html document, hence no boilerplate is needed in animals.ejs.

### Controllers & Express Router
Controllers are an important organizational tool when apps start to have several views. So far we have placed all our routes into `index.js`. This can get cumbersome when dealing with a large number of routes. Say we have a website for our various likes and dislikes separated by category. We can start by lumping the .ejs likes pages into their own directory, as in `./likes/foods` and the dislikes into `./dislikes/foods`. Then we group related routes together in separate files inside of a `controllers` directory. For example `controllers/likes` and `controllers/dislikes`. Each file can contain multiple routes to different content, but they are similar enough to be grouped together. Below, the `app` object does not exist. Instead we use Express' `router()` method that will wrap the routes into a module that we'll export back into our main server file `index.js`.
```js
// controllers/likes
const express = require('express');
const router = express.Router();

router.get('/foods', (req, res)=> {
    res.render('likes/food', {title: 'Foods I Like', foods: ['hamburgers', 'corn', 'century eggs']});
});

router.get('/animals', (req, res)=> {
    res.render('likes/animals', {title: 'Animals I Like', animals: ['cats', 'bullfrogs', 'axolotls']});
})
```
In order for the above routes to work, we need to add more Middleware into our `index.js` file:
```js
// index.js
const express = require('express');
const app = express();
const ejsLayouts = require('express-ejs-layouts');

app.set('view engine', 'ejs');
app.use(ejsLayouts);

app.use('/likes', require('.controllers/likes'));
```
Now we do not need to specify `/likes/animals` or `/likes/foods` in our URL pattern, because the Middleware is making our `controllers/likes` route do the rest. 

### CRUD (Create Update Read Delete)

### ORM (Object Relational Mappers)
An Object Relational Mapper creates a 'map' that represents databases via JavaScript objects. When we use an ORM we can use JavaScript to CRUD data instead of writing SQL queries. 

#### Sequelize
*Sequelize* is an ORM written in JavaScript that is used in Node.js to work with relational databases like Postgres. With sequelize we can represent `models` (tables) and the data inside of them, represent how models are associated with eachother, validate data prior to adding to the database, and perform general database tasks in an OO fashion. 

#### Modles
A *Model* is a JS object that maps to a table (data relation) in a database. A model is kind of like the blueprint for each row of data contained in the table. Each instance of a model represents a row of data. 