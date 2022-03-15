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


# CLI

<br><br>


## Run node script from bash script and output logs
```bash
node '../convert.js' 2>&1
```

<br><br>


## Run ESM file with eval
- To be able to use import with eval we must the --input-type=module flag
```bash
node --input-type=module -e "import {CSV2JSON} from '../convert.js'; CSV2JSON('$DUMB', '$editDumb', '$headers')"
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

// load global environment variables from .env file and from specific one too
const path = `${__dirname}/../.env-files/${process.env.npm_lifecycle_event}.env`
dotenv.config({silent: true, path: path})// load script specific env vars from .env-files folder
dotenv.config({silent: true}) // load global env vars from .env file


```

<br><br>

#### global
```bash
npm config set NODE_SKIP_PLATFORM_CHECK 1
```




































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

## Profiling

#### Bubbleprof
- https://www.youtube.com/watch?v=tq3mqrV49l8
- 
```javascript
npm_lifecycle_event=start-dev clinic bubbleprof --on-port 'autocannon -c 5 -a 500 localhost:1337' -- node server.js
```










































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


#### start script with paramater
```bash
node app.js a=1
```
```javascript
// method #1
var args = process.argv.slice(2);
// [ 'a=1' ]



// method #2
var argv = require('minimist')(process.argv.slice(2));
console.dir(argv);
# $ node example/parse.js --apple beep --banana boop
# { _: [], apple: 'beep', banana: 'boop' }




// method #3
node -e "require('../convert.js').CSV2JSON('$DUMB', '$editDumb', '$headers')"
```


























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Directory

## Get current working directory
```javascript
console.log( 'Current working directory: ' + __dirname );
```

<br><br>


## Get path of root from project
```javascript
var path = require('path');
var appDir = path.dirname(require.main.filename);
```






<br><br>
<br><br>



## Exit script
```javascript
process.exit();
```

<br><br>
<br><br>



## Prevent script from closing on Error
```javascript
process.on('uncaughtException', function (err) {
  console.log('Caught exception: ' + err);
});
```
























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



 # Exec
 
 <br><br>

 
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


## Show realtime terminal logs
```javascript
// ----- TERMINAL ----
import util from 'util'
import childProcess from 'child_process'
const exec = util.promisify(childProcess.exec)
const spawn = childProcess.spawn

const scriptPath = '../file.sh'




// Method #1
# You may must use ...process.env or you will get node command not found when executing node apps inside of your bash script
const child = spawn('bash', [scriptPath], {stdio: 'inherit', env: { ...process.env, MONGOURI: process.env.MONGOB_ADDRESS}}})

child.on('close', code => {
    log('child process exited with code: ' + code)
})






// Method #2
it.only('should convert dumb and import it to database', done => {
    try {
        # You may must use ...process.env or you will get node command not found when executing node apps inside of your bash script
        const child = spawn('bash', [scriptPath], {env: { ...process.env, MONGOURI: process.env.MONGOB_ADDRESS}}})

        child.stdout.on('data', line => {
            log('stdout: ' + line)
        })

        child.stderr.on('data', e =>{
            log('stderr: ' + e)
        })

        child.on('close', code => {
            log('child process exited with code: ' + code)
            done()
        })
    } catch (e) {
        console.error(e) // should contain code (exit code) and signal (that caused the termination).
    }
})
```



<br><br>


## Run as sudo
```javascript
const child = spawn('sh', ['-c', `echo abc | sudo -S bash -c '${command}'`], {
    cwd: process.cwd(),
    detached: true,
    stdio: 'inherit'
})
```


























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# NPM
https://docs.npmjs.com/cli/config

<br><br>

## Install

<br><br>

#### Install module
```bash
npm i request
```

<br><br>

#### Install module global
```bash
npm i request -g
```

<br><br>

#### Use module from node_modules folder instead of the global one
```bash
npx nodemon
```

<br><br>

#### Install module from github
```bash
npm install florianholzapfel/express-restify-mongoose#master
# npm install florianholzapfel/express-restify-mongoose#commit-hash-here
```

<br><br>

#### Install module as dev dependency
```bash
npm i -D nyc
```


<br><br><br><br>


## Update

<br><br>

#### Update modules
```bash
# run inside of your project root to update all modules
npm update

# update specific module
npm update browser-sync
```


<br><br>><br><br>


## scripts

<br><br>

## Execute test
```javascript
"scripts": {
  "test": "mocha test.js"
}
```
```bash
npm test
```

<br><br>

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



<br><br>><br><br>


## cache

<br><br>

## clear cache
```javascript
npm cache clean --force
```






























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# FS

## fs promise with ESM
```javascript
// method #1 - Use fs-extra

// method #2
import {default as fsWithCallbacks} from 'fs'
const fs = fsWithCallbacks.promises
```



<br><br>

## check if file exists
```javascript
//sync 
if(await fs.exists('./bookmarks.txt')) {
 //..
}
```



<br><br>

## read directory and get all file names

```javascript
//sync 
const dirname = `${process.cwd()}/test/test-db-dumps`
const files = await fs.readdir(dirname)
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
const fs = require('fs').promises
const path = `${__dirname}/img.png`

comstimageBuffer = await fs.readFile(path)

const img = await fs.readFile(path, "binary")
const imageUint8Array = Buffer.from(img)
```


<br><br>

## Read vanilla javascript file
```javascript
eval(require('fs').readFileSync('./website/js/req.js', 'utf8'));
```




<br><br>

## Read file by reading each line
```javascript
// method #1
var lineReader = require('line-reader');

await new Promise((resolve, reject) => {
    let counter = 0
    lineReader.eachLine(dumb, async(line, last) => {
        counter++

       log(`line before convert: ${line}`)
       let json = (
           await csv(options).fromString(headers + '\n\r' + line)
               .preFileLine((fileLineString, lineIdx) => {
                   if (fileLineString.match(/'.*'''|'.*''/)) {
                       // eslint-disable-next-line max-len
                       console.log(`Line #${lineIdx + 1} is invalid. We will try to fix it now by usign regex. Invalid Line: ${fileLineString}`)
                       fileLineString = fileLineString.replace(/'''|''/g, '\'')
                   }

                   return fileLineString
               })
               .on('error', (err)=>{
                   console.log(err)
               })
       )[0]
       json = JSON.stringify(json).replace(/\\"/g, '')
       log(`line after convert: ${json}`)

        if (last) {
            // Check for last Line
            json = `${json}]`
        } else if (counter === 1) {
            // Check for first line
            json = `[${json},\n\n`
        } else {
            // Check for between line
            json = `${json},\n\n`
        }

        await fs.appendFile(editDumb, json)

        if (last) {
            resolve()
        }
    })
})
```




<br><br>

## Read only first line of file
```javascript
// method #1
 const firstline = require('firstline')
const line = await firstline(pathToFile)






// method #2
const fs = require('fs');
const readline = require('readline');

async function getFirstLine(pathToFile) {
  const readable = fs.createReadStream(pathToFile);
  const reader = readline.createInterface({ input: readable });
  const line = await new Promise((resolve) => {
    reader.on('line', (line) => {
      reader.close();
      resolve(line);
    });
  });
  readable.close();
  return line;
}







// method #3
await new Promise(resolve => {
      const rl = readline.createInterface({
          input: fs.createReadStream(pathToFile),
      })

      rl.on('line', function (line) {
          console.log(line);
          resolve(line)
      })
  })
```









<br><br> <br><br>


## write file
```javascript                                           
await fs.writeFile('filename.txt', 'test');
```


<br><br>

## write file to json
```javascript                                           
await fs.writeJson('filename.txt', jsonArray)
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

# stream

<br><br>

## create stream
```javascript
const readStream = fs.createReadStream('data.csv')
const writeStream = fs.createWriteStream("outpuData.json")
const stream = readStream.pipe(writeStream)
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








































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


#### Convert csv to json (works with large files)
```javascript
/*
███████████████████████████████████████████████████████████████████████████████
██******************** PRESENTED BY t33n Software ***************************██
██                                                                           ██
██                  ████████╗██████╗ ██████╗ ███╗   ██╗                      ██
██                  ╚══██╔══╝╚════██╗╚════██╗████╗  ██║                      ██
██                     ██║    █████╔╝ █████╔╝██╔██╗ ██║                      ██
██                     ██║    ╚═══██╗ ╚═══██╗██║╚██╗██║                      ██
██                     ██║   ██████╔╝██████╔╝██║ ╚████║                      ██
██                     ╚═╝   ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝                      ██
██                                                                           ██
███████████████████████████████████████████████████████████████████████████████
███████████████████████████████████████████████████████████████████████████████
*/
import 'ErrorManager'
import log from 'fancy-log'
import csv from 'csvtojson'
import fs from 'fs-extra'
import lineReader from 'line-reader'

import { __dirname } from '../../../utils.js'

const CSV2JSON = async(dumb, editDumb, headers, {
    options = {
        trim: true,
        delimiter: '|',
        quote: '"',
        escape: '"',
        fork: true
    }
} = {}) => {
    try {
        log(`\n\nStarting CSV2JSON - Current directory: ${__dirname()} - Please wait..`)

        if (!dumb && !editDumb && !headers) {
            var {dumb, editDumb, headers} = require('minimist')(process.argv.slice(2))
        }

        if (!dumb && !editDumb && !headers) {
            throw new BaseError('Param is missing CSV2JSON()')
        }

        options = {
            ...options,
            headers: Array.isArray(headers) ? headers : JSON.parse(headers)
        }

        await new Promise((resolve, reject) => {
            let firstLine, counter = 0
            lineReader.eachLine(dumb, async(line, last) => {
                counter++

                firstLine
                if (counter === 1) {
                    firstLine = line
                }

                // log(`line before convert: ${line}`)
                let json = (
                    await csv(options).fromString(headers + '\n\r' + line)
                        .preFileLine((fileLineString, lineIdx) => {
                            // eslint-disable-next-line max-len
                            if (counter !== 1 && !fileLineString.match(/^(?:[^"\\]|\\.|"(?:\\.|[^"\\])*")*$/g)) {
                                // eslint-disable-next-line max-len
                                console.log(`Line #${lineIdx + 1} is invalid. It has unescaped quotes. We will skip this line.. Invalid Line: ${fileLineString}`)
                                fileLineString = ''
                            }

                            return fileLineString
                        })
                        .on('error', e => {
                            e = `Error while converting CSV to JSON.
                            Line before convert: ${line}
                            Error: ${e}`
                            throw new BaseError(e)
                        })
                )[0]

                // log(`line after convert: ${json}`)

                if (json) {
                    json = JSON.stringify(json).replace(/\\"/g, '')

                    // double verify if there is an unclosed quote
                    if (json.match(/^(?:[^"\\]|\\.|"(?:\\.|[^"\\])*")*$/g)) {
                        if (last) {
                            // Check for last Line
                            json = `${json}]`
                        } else if (counter === 1) {
                            // Check for first line
                            json = `[${json},\n\n`
                        } else {
                            // Check for between line
                            json = `${json},\n\n`
                        }

                        await fs.appendFile(editDumb, json)
                    }
                }

                if (last) {
                    resolve()
                }
            })
        })
    } catch (e) {
        throw new BaseError(`Error while converting CSV to JSON - Error: ${e}`)
    }
}

export { CSV2JSON }






















// method #2 - sync
const csvConverter = new csv(options)

const readStream = fs.createReadStream(dumb)
const writeStream = fs.createWriteStream(editDumb)
readStream.pipe(csvConverter).pipe(writeStream)






// method #3 - from Stream
const readStream = fs.createReadStream('yourFile.csv')

await new Promise((resolve, reject) => {
    csv(options)
        .fromStream(readStream)
        .subscribe(json=>{
            return new Promise((resolve, reject)=>{
                resolve(json)
            })
        })
        .on('header', header => {
            log(`Converting CSV to JSON - headers: ${headers}`)
        })
         .on('data', async data => {
                // data is a buffer object
                const jsonStr = data.toString('utf8')
                log(`Converting CSV to JSON - Current Line: ${jsonStr}`)
                await fs.appendFile(editDumb, jsonStr)
            })
        .on('done', () => {
            log('Finished converting CSV to JSON')
            resolve()
        })
        .on('error', e => {
            throw new BaseError(`Error while converting CSV to JSON - Error: ${e}`)
        })
})
```


