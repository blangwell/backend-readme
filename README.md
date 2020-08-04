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
