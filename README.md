# Node.js Cheat Sheet
Node.js Cheat Sheet with the most needed stuff..

<br />
<br />


## npm ERR! code ENOENT

#### windows (change the value of Start to 1.)
HKEY_LOCAL_MACHINE -> SYSTEM -> ControlSet001 -> Services -> Null




<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


# package.json

## General Informations
- https://docs.npmjs.com/creating-a-package-json-file


## Create package.json file of already existing project.
```bash
npm init --yes
```


## Create custom license
```javascript
// Then include a file named <filename> at the top level of the package.
{ "license" : "SEE LICENSE IN <filename>" }
```

## set/install latest version of dependencies
```javascript
/* method 1* - run npm update after this/
  "dependencies": {
    "express": "*",
    "mongodb": "*",
    "underscore": "*",
    "rjs": "*",
    "jade": "*",
    "async": "*"
  }
  
/* method 2*/
"dependencies":{
    "foo" : ">=1.4.5"
}
```



## Use nodemon with test
```javascript
"scripts": {
  "test": "mocha test.js",
  "test-watch": "nodemon --exec \"npm test\""
}
```
```bash
npm run test-watch
```


<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

## environment variable

#### project

Create file config.env in root of project. In this file you can create environment variables. Then install:
```bash
npm i dotenv
```

Then use in your project:
```javascript
const dotenv = require('dotenv');
dotenv.config({ path: './config.env' });
```

#### global
```bash
npm config set NODE_SKIP_PLATFORM_CHECK 1
```

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


## Get current working directory
```javascript
console.log( 'Current working directory: ' + __dirname );
```

## Exit script
```javascript
process.exit();
```

<br />
<br />


 _____________________________________________________
 _____________________________________________________
 
 
 # Exec
 
 
## Run terminal command
```javascript
// sync
const { exec } = require('child_process');
exec('cat *.js bad_file | wc -l', (err, stdout, stderr) => {
  if (err) {
    // node couldn't execute the command
    return;
  }

  // the *entire* stdout and stderr (buffered)
  console.log(`stdout: ${stdout}`);
  console.log(`stderr: ${stderr}`);
});



//async
const util = require('util');
const exec = util.promisify(require('child_process').exec);

async function lsExample() {
  try {
    const { stdout, stderr } = await exec('ls');
    console.log('stdout:', stdout);
    console.log('stderr:', stderr);
  } catch (e) {
    console.error(e); // should contain code (exit code) and signal (that caused the termination).
  }
}
```

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />



# NPM
https://docs.npmjs.com/cli/config


## Create package.json
```bash
npm init
```



## Update modules
```bash
# run inside of your project root to update all modules
npm update

# update specific module
npm update browser-sync
```


<br />
<br />

## Execute test
```javascript
"scripts": {
  "test": "mocha test.js"
}
```
```bash
npm test
```

## Execute custom scripts key
```javascript
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "custom_test": "some code here.."
}
```
```bash
npm run custom_test
```


<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# FS

## fs promise with ESM
```javascript
import {default as fsWithCallbacks} from 'fs'
const fs = fsWithCallbacks.promises
```
<br><br>

## read file

```javascript
//sync 
fs.readFile('./bookmarks.txt', 'utf-8', function read(e, data) {
   if (e) {
       log('Error while reading bookmarks file: ' + e);
        return;
    }
    log('Successfully open boomarks file (raw): ' +  data);
});

//async
const fs = require('fs').promises;
async function loadMonoCounter() {
    const data = await fs.readFile("monolitic.txt", "binary");
    return new Buffer(data);
}
```

## Read vanilla javascript file
```javascript
eval(require('fs').readFileSync('./website/js/req.js', 'utf8'));
```

<br><br>

## write file
```javascript                                           
await fs.writeFile('filename.txt', 'test');
```




<br><br>


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# request
Since request is outdated you can also use:
- https://www.npmjs.com/package/axios



## POST with JSON
```javascript
const options = {
  'method': 'POST',
  'url': 'http://example.com/tree/item', // url or uri
  'headers': {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({"name":"Fortnite","parent_id":15,"sorting":5})

};

// ASYNC request
const request = require("request-promise");
const r = await request(options);
log( 'addItem() - response: ' + JSON.stringify(r, null, 4) );

// SYNC
var request = require('request');
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});


// ASYNC axios method
 const res = await axios.post(  window.location.origin + '/secure', { client_id: 'a', client_secret: 'b'  }, {
            headers: { authorization: accessToken.data['access_token'] }
 });
```


## DELETE
```javascript
const options = {
  'method': 'DELETE',
  'url': 'http://example.com/tree/item?id=11'  // url or uri
};
```

## GET
```javascript
// ASYNC axios
const response = await axios.get('/user?ID=12345');
```


## Check Proxy
```javascript
const options = {
      proxy: "http://" + proxy, // http or https
      url: "http://www.google.com",  // url or uri
      timeout: 10000
 };
```
## Public Proxies
- https://proxyscrape.com/free-proxy-list
- https://openproxy.space/list

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Difference between setImmediate and process.nextTick

```javascript
setImmediate(() => {
console.log('#3 - last after all callbacks are done..');
});

process.nextTick(() => {
console.log('#2 - Will come after next tick..');
});

console.log( '#1 -will come first..' );
```





































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Remote Debugging (https://nodejs.org/en/docs/guides/debugging-getting-started/)
- Allows you to debug your node.js code via Remote on your Browser

<br><br>

1. Execute the js file you want to debug with this command:
```javascript
node --inspect-brk test/config/test.js
```

<br><br>
2. Open your Browser
<br>2.1 Chrome:
- chrome://inspect/#devices
- Make sure that your Network Targets allows **localhost:9229** (will be default in most cases)

<br><br>
3. Click on **Inspect**. Thats it! Now you can debug your Node Code within your Browser Inspector Console.

