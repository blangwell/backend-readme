# Backend Notes

## Node
Node is a a framework that allows us to use JavaScript to write server-side logic.

### Installing Node
I used Homebrew to get Node installed. I updated Homebrew with `brew update` and installed Node with `brew install node`.
### Creating a New Node App 
I created a new directory with `mkdir my-first-node-proj`, `cd`'d into the directory and initialized Node with the command `npm init`. _You can bypass the init steps and use the default JSON values by using the `-y` flag after `npm init`._
I used the default `main` value from `package.json` and used `touch index.js` as the entry point to the application. To run the program, from the project directory in terminal I ran `node index.js`.

### Summary
To create a new Node app:
```js
npm init // initialize Node in cwd
touch index.js // create entry point
node index.js // run file in Node
```