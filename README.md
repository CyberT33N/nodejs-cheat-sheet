# Node.js Cheat Sheet
Node.js Cheat Sheet with the most needed stuff..

<br />
<br />


## npm ERR! code ENOENT

#### windows (change the value of Start to 1.)
HKEY_LOCAL_MACHINE -> SYSTEM -> ControlSet001 -> Services -> Null




























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# package.json

## General Informations
- https://docs.npmjs.com/creating-a-package-json-file


<br><br>

## Create package.json file of already existing project.
```bash
npm init --yes
```

<br><br>

## Create custom license
```javascript
// Then include a file named <filename> at the top level of the package.
{ "license" : "SEE LICENSE IN <filename>" }
```

<br><br>

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

<br><br>

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






<br><br>

## version

<br><br>

#### Install latest patch version
- If you see ~1.0.2 it means to install version 1.0.2 or the latest patch version such as 1.0.4.
```javascript
"request": "~1.0.2"
```
<br><br>

#### Install latest minor or patch version
- If you see ^1.0.2 it means to install version 1.0.2 or the latest minor or patch version such as 1.1.0.
```javascript
"request": "^1.0.2"
```





































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

## environment variable

#### project
```javascript
const dotenv = require('dotenv')

// load global environment variables from .env file
dotenv.config({silent: true})
console.log(process.env)

// 1 liner - load global environment variables from .env file
require('dotenv').config()

// load specific file
dotenv.config({ path: './config.env' });
```

#### global
```bash
npm config set NODE_SKIP_PLATFORM_CHECK 1
```











































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


## Get current working directory
```javascript
console.log( 'Current working directory: ' + __dirname );
```

## Exit script
```javascript
process.exit();
```


























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



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



































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


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
<br><br>


# Assert Methods (https://www.w3schools.com/nodejs/ref_assert.asp)

<br><br>


## .assert() (- https://www.w3schools.com/nodejs/met_assert.asp)
- The assert() method tests if a given expression is true or not. If the expression evaluates to 0, or false, an assertion failure is being caused, and the program is terminated. The assert() method is an alias of the assert.ok() method.


<br><br>

Syntax:
```javascript
assert(expression, message);
```

<br><br>

```javascript
var assert = require('assert');
assert(50 > 70, "My message goes here");

// The assert method also throws an error if the expression evaluates to 0:
var assert = require('assert');
assert(50 - 50);
```




































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# request
Since request is outdated you can also use:
- https://www.npmjs.com/package/axios
- https://nodejs.org/api/https.html


## axios
- https://github.com/CyberT33N/axios-cheat-sheet/edit/main/README.md


<br><br>


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

// callback - request
var request = require('request');
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

// await - request-promise
const request = require("request-promise");
const r = await request(options);
log( 'addItem() - response: ' + JSON.stringify(r, null, 4) );













/* callback - https node.js module */
// stringify body for https module POST request
const host = 'example.com'
const path = 'foo.bar'
const method = 'POST'
const cookie = 'myCookie=123'
const port = 443
const body = {
    name: "abc"
}
const headers = {
    "Content-Type": "application/json",
    "Set-Cookie": cookie,
}
            
const data = JSON.stringify(body)
const options = {
    hostname: host,
    port: port,
    path: `/${path}`,
    method: method,
    headers: {
        ...headers,
        'Content-Length': data.length
    },
}

const req = https.request(options, res => {
    let resBody = ''
    res.on('data', function (chunk) {
        resBody += chunk
    })

    res.on('end', function () {
        resBody = JSON.parse(resBody)
        expect(res.statusCode).to.equal(201)
        expect(res.headers.cookies).to.equal(cookie)
        expect(resBody).to.deep.equal(body)
        done()
    })
})

req.on('error', e => {
    console.error(e)
    throw new Error(e);
})

req.write(data)
req.end()
```




<br><br>


## DELETE
```javascript
const options = {
  'method': 'DELETE',
  'url': 'http://example.com/tree/item?id=11'  // url or uri
};
```



<br><br>



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









































































<br><br>
_________________________________________________
_________________________________________________
<br><br>


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

# events (https://nodejs.org/api/events.html)





<br><br>

## .on & .once
```javascript
// .on (every time the listener gets fired)
browser.on('disconnected', coolFunctionHere)

// .once (the listener will only 1 time get fired)
browser.once('disconnected', coolFunctionHere)
```

## .off
- Make sure that you specify from which function you want to delete the event listener. 
```javascript
const boundOnDisconnect = () => _onDisconnect()
browser.once('disconnected', boundOnDisconnect)
        
this.browser.off('disconnected', boundOnDisconnect)
```



this.browser.off('disconnected', this.boundOnDisconnect)





<br><br>

## EventEmitter

<br><br>

#### Wait for event callback to be finished
```javascript
/*---- METHOD #1 - extends class with EventEmitter ----*/

// PuppeteerService.js
const EventEmitter = require('events')

module.exports = class PuppeteerService extends EventEmitter {
    constructor(){
        super()
        framework.on('reconnect', () => this.emit('reconnect'))
    }
    
    async _reconnect() {
        this.browser = await puppeteer.launch()
        
        this.boundOnDisconnect = () => this._onDisconnect()
        this.browser.on('disconnected', this.boundOnDisconnect)
        
        this.emit('reconnect')
    }
    
    async _onDisconnect() {
        await new Promise(resolve => setTimeout(resolve, 1000)); // <-- verify that we will wait..
        await this._reconnect()
    }
}

const framework = new PuppeteerService()






// service.test.js
const PuppeteerService = require('./PuppeteerService')
const puppeteerService = new PuppeteerService()

it('should create new browser object after close', async () => {
    await puppeteerService.browser.close() // <-- will fire disconnect listener

    return new Promise((resolve, reject) => {
        puppeteerService.on('reconnect', async () => {
            try{
                const checkConnection = await puppeteerService.browser.isConnected()
                expect(checkConnection).to.equal(true)
                resolve()
            } catch(e){
                reject()
            }
        })
    })
})





























/*---- METHOD #2 - create new variable with EventEmitter ----*/

// PuppeteerService.js
const EventEmitter = require('events')

module.exports = class PuppeteerService {
   /**
     * Create EventEmitter
     */
    constructor() {
        this.em = new EventEmitter();
        ['on', 'off', 'once'].forEach(m => this[m] = this.em[m])
    }

    async _reconnect() {
        this.browser = await puppeteer.launch()
        
        this.boundOnDisconnect = () => this._onDisconnect()
        this.browser.on('disconnected', this.boundOnDisconnect)
        
        this.em.emit('reconnect')
    }
    
    async _onDisconnect() {
        await new Promise(resolve => setTimeout(resolve, 1000)); // <-- verify that we will wait..
        await this._reconnect()
    }
}





// service.test.js
const PuppeteerService = require('./PuppeteerService')
const puppeteerService = new PuppeteerService()

it('should create new browser object after close', async () => {
    await puppeteerService.browser.close() // <-- will fire disconnect listener

    return new Promise((resolve, reject) => {
        puppeteerService.em.on('reconnect', async () => {
            try{
                const checkConnection = await puppeteerService.browser.isConnected()
                expect(checkConnection).to.equal(true)
                resolve()
            } catch(e){
                reject()
            }
        })
    })
})
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

































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# require

<br><br>

## folder (http://nodejs.org/api/modules.html#modules_folders_as_modules)
- When you pass a folder to Node's require(), it will check for a package.json for an endpoint. If that isn't defined, it checks for index.js, and finally index.node (a c++ extension format). So the index.js is most likely the entry point for requiring a module.
```javascript
// index.js
exports.something = require("./routes/something.js");
exports.others = require("./routes/others.js");
```




<br><br>

## use global
```javascript
// app.js
require('./test')
console.log(test) // true

// test.js
global.test = true
```










































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# DNS

<br><br>

## lookup
```javascript
const dns = require('dns')
async function lookupPromise(){
    return new Promise((resolve, reject) => {
        dns.lookup(host, (err, address, family) => {
            if(err) reject(err)
            resolve(address)
        })
   })
}
```















































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# buffer

<br><br>

## Uint8 Array to Base64
```javascript
var b64 = Buffer.from(u8).toString('base64')
```

<br><br>

## Check if buffer
```javascript
var check = Buffer.isBuffer(readableFileStreamOrBuffer)
```

<br><br>

## Create Buffer
```javascript
const buff = Buffer.from(readableFileStreamOrBuffer)
```




