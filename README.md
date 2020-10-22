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
```bash
// Then include a file named <filename> at the top level of the package.
{ "license" : "SEE LICENSE IN <filename>" }
```

## set/install latest version of dependencies
```bash
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


## Update modules
```bash
# run inside of your project root to update all modules
npm update

# update specific module
npm update browser-sync
```




<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# FS

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

## write file
```javascript                                           
await fs.writeFile('filename.txt', 'test');
```


<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# request


## POST
```javascript
const options = {
  'method': 'POST',
  'url': 'http://example.com/tree/item',
  'headers': {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({"name":"Fortnite","parent_id":15,"sorting":5})

};

// ASYNC
const request = require("request-promise");
const r = await request(options);
log( 'addItem() - response: ' + JSON.stringify(r, null, 4) );

// SYNC
var request = require('request');
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
```

## Check Proxy
```javascript
const request = require("request-promise");
async function checkproxyStatus(proxy){

          const options = {
            proxy: "http://" + proxy,
            uri: "http://www.google.com",
            timeout: 10000
          };

         try{
             if( await request(options) ) return "http://" + proxy;
         }
         catch (e) {
             console.log( 'checkproxyStatus() - error: ' + e );
         }


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
