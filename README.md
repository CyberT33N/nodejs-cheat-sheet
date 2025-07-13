e# Node.js Cheat Sheet
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



# Shebang
- Tells the operating system to use the node program to run the script.
Now, you can run JavaScript code with ./fileName.js instead of node fileName.js.
```javascript
#!/usr/bin/env node 
```










<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Security

## Helmet
- Help secure Express apps by setting HTTP response headers.
- https://www.npmjs.com/package/helmet





















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Validation

## Schema
- https://www.npmjs.com/package/zod
  - TypeScript-first schema validation with static type inference 






















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# Benchmark
- https://www.npmjs.com/package/mitata









<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# Auth
- https://www.passportjs.org





















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# CLI

<details><summary>Click to expand..</summary>

<br><br>


## Start with more detailed logs
```bash
node app.js --trace-deprecation
```


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


</details>






































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Performance


<details><summary>Click to expand..</summary>

<br><br>

## memoizee

<br><br>

- **DO NOT memoize a function and then use an object as param or caching will not work:
```javascript
const memoize = require('memoizee')
const memProfile = require('memoizee/profile')

function getCacheInfo() {
    const { initial, cached } = memProfile.statistics[Object.keys(memProfile.statistics)[0]]
    return { initial, cached }
}

function objectToString(obj) {
    return JSON.stringify(obj)
}

const memoizedObjectToString = memoize(objectToString)

memoizedObjectToString({ a: 1, b: 2 })
memoizedObjectToString({ a: 1, b: 2 })
memoizedObjectToString({ a: 1, b: 2 })
memoizedObjectToString({ 'foo': 'bar' })
memoizedObjectToString({ 'foo': 'bar' })
memoizedObjectToString({ 'foo': 'bar' })

const { initial, cached } = getCacheInfo()

console.log(`memoizee is caching ${initial} different values. ${cached} out of ${initial+cached} results were returned from the cache.`)
```


If you still want to use objects as param then use the normalizer option
```javascript
const memoize = require('memoizee')
const memProfile = require('memoizee/profile')

function getCacheInfo() {
    const { initial, cached } = memProfile.statistics[Object.keys(memProfile.statistics)[0]]
    return { initial, cached }
}

function objectToString(obj, test) {
     console.log('test: ', test)
    return JSON.stringify(obj)
}

const memoizedObjectToString = memoize(objectToString, {
    normalizer: args => JSON.stringify(args[0])
})

result = memoizedObjectToString({ a: 1, b: 2 }, 1)
result = memoizedObjectToString({ a: 1, b: 2 }, 1)
result = memoizedObjectToString({ a: 1, b: 2 }, 1)
result = memoizedObjectToString({ 'foo': 'bar' }, 1)
result = memoizedObjectToString({ 'foo': 'bar' }, 1)
result = memoizedObjectToString({ 'foo': 'bar' }, 1)

const { initial, cached } = getCacheInfo()

console.log(`memoizee is caching ${initial} different values. ${cached} out of ${initial+cached} results were returned from the cache.`)
```





<br><br>
<br><br>
<br><br>
<br><br>

## memoize-one (https://www.npmjs.com/package/memoize-one)
```bash
// memoize-one uses the default import
import memoizeOne from 'memoize-one';

function add(a, b) {
  return a + b;
}
const memoizedAdd = memoizeOne(add);

memoizedAdd(1, 2);
// add function: is called
// [new value returned: 3]

memoizedAdd(1, 2);
// add function: not called
// [cached result is returned: 3]

memoizedAdd(2, 3);
// add function: is called
// [new value returned: 5]

memoizedAdd(2, 3);
// add function: not called
// [cached result is returned: 5]

memoizedAdd(1, 2);
// add function: is called
// [new value returned: 3]
// 👇
// While the result of `add(1, 2)` was previously cached
// `(1, 2)` was not the *latest* arguments (the last call was `(2, 3)`)
// so the previous cached result of `(1, 3)` was lost
```

</details>


























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

## environment variable

<details><summary>Click to expand..</summary>

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


</details>

















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

## Profiling

<details><summary>Click to expand..</summary>

<br><br>

## Clinic.js
```shell
npm i autocannon clinic -g
```




<br><br>
<br><br>

#### Bubbleprof
- https://www.youtube.com/watch?v=tq3mqrV49l8

<br><br>
  
```javascript
npm_lifecycle_event=start-dev clinic bubbleprof --on-port 'autocannon -H "project-id: 7677" -c 5 -a 20 /api/Templates' -- node server.js
```
- It automatically detect host and port

</details>










































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


## start script with paramater
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

<details><summary>Click to expand..</summary>

## Get current working directory
```javascript
console.log( 'Current working directory: ' + __dirname );
```


<br><br>

## Get current file path
```javascript
// Method #1
module.filename

// Method #2
console.log( 'Current file path: ' + __filename );
```

<br><br>
<br><br>


## Get relative file path
```javascript
 const relativePath = path.relative(process.cwd(), module.filename)
```

<br><br>
<br><br>

## Get relative file path for parent package.json
```javascript
/**
* 
* @param {*} filePath 
* @returns 
*/
function findMainProjectDirectory(filePath) {
  // Starte mit dem Verzeichnis der aktuellen Datei
  let currentDir = path.dirname(filePath);

  // Gehe so weit zurück, bis die package.json gefunden wird
  while (!fs.existsSync(path.join(currentDir, 'package.json'))) {
    currentDir = path.dirname(currentDir);

    // Wenn wir das Wurzelverzeichnis erreichen, brechen wir ab
    if (currentDir === path.dirname(currentDir)) {
      throw new Error('Hauptprojektverzeichnis mit package.json nicht gefunden!');
    }
  }

  return currentDir;
}

const mainProjectDir = findMainProjectDirectory(module.filename)
const relativePath = path.relative(mainProjectDir, module.filename)
```

<br><br>
<br><br>


## Get path of root from project
```javascript
// Method #1
process.cwd()


// Method #2
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





</details>















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# process
- https://nodejs.org/api/process.html


<br><br>

## process.stdout

<br><br>

### process.stdout.write
- Write something directly to the terminal where you node.js app is running
```typescript
// Take screenshots at high framerate but play back at real-time speed
const totalFrames = 150
const progressBarWidth = 30

for (let i = 0; i < totalFrames; i++) {
    const percent = Math.floor((i / totalFrames) * 100)
    const filled = Math.floor((i / totalFrames) * progressBarWidth)
    const empty = progressBarWidth - filled
    const progressBar = '█'.repeat(filled) + '▒'.repeat(empty)
    process.stdout.write(`\r📸 Creating Screenshots: ${progressBar} ${percent}% (${i}/${totalFrames}) `)
    
    await page.screenshot({ path: join(tmpDir, `screenshot-${i}.png`) })
    await new Promise<void>(resolve => setTimeout(resolve, 100)) // Real-time capture
}

console.log('\n✨ Screenshots captured! Converting to GIF...')
```









<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



 # Exec

<details><summary>Click to expand..</summary>
 
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
// Method #0
let ls
const { spawn } = require('child_process');

function executeCommand(command) {
  return new Promise((resolve, reject) => {
    // you can use ls.kill to kill the terminal
    ls = spawn(command, { shell: true });

    ls.stdout.on('data', (data) => {
      console.log('stdout:', data.toString());
    });

    ls.stderr.on('data', (data) => {
      console.error('stderr:', data.toString());
    });

    ls.on('error', (error) => {
      reject(error);
    });

    ls.on('close', (code) => {
      if (code === 0) {
        resolve();
      } else {
        const error = new Error(`Command failed with code ${code}`);
        error.code = code;
        reject(error);
      }
    });
  });
}

async function startDevServer() {
  try {
    await executeCommand('cd ~/Projects/gitlab/servers/ccs && npm run start-dev');
  } catch (error) {
    console.error(error);
  }
}

startDevServer();













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


Option 1:
- Will ask for password. You can use this aswell in electron.js
```javascript
const sudo = require('sudo-prompt');

const sudoOptions = {
    name: 'Secure File Vault'
};

// Promisified sudo exec
function sudoExec(command) {
    return new Promise((resolve, reject) => {
        sudo.exec(command, sudoOptions, (error, stdout, stderr) => {
            if (error) reject(error);
            else resolve(stdout);
        });
    });
}

await sudoExec(`veracrypt --text --create "${containerPath}" --size "${containerSize}" --password "${password}" --encryption AES --hash sha512 --filesystem FAT --non-interactive`);
```

Option 2:
```javascript
const child = spawn('sh', ['-c', `echo abc | sudo -S bash -c '${command}'`], {
    cwd: process.cwd(),
    detached: true,
    stdio: 'inherit'
})
```




<br><br>
<br><br>


## Wait for specific text in stdout
```typescript
**
 * 🎬 Start Next.js
 * @async
 * @function startNextJs
 * @description Starts the Next.js development server
 * @returns {Promise<ChildProcess>} Next.js process instance
 */
const startNextJs = async(): Promise<ChildProcess> => {
    console.log('Starting Next.js server...')

    try {
        await killPort(3000)
        
        return new Promise((resolve, reject) => {
            const nextProcess: ChildProcess = exec('npm run dev', { cwd: process.cwd() })

            nextProcess.stdout?.on('data', (data: string) => {
                if (data.includes('Ready in')) {
                    resolve(nextProcess)
                }
            })

            nextProcess.stderr?.on('data', (err: string) => reject(new Error(err)))
        })
    } catch (error) {
        console.error('Error starting Next.js server:', error)
        throw error
    }
}
```






</details>





















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# NPM
https://docs.npmjs.com/cli/config


<details><summary>Click to expand..</summary>

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






</details>























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# FS

<details><summary>Click to expand..</summary>

## check if path is folder
```javascript
const fs = require('fs-extra')

async getDirectories(path) {
     const dirContent = await fs.readdir(path)
     const directories = []

     for (const fileOrFolder of dirContent) {
         const pathFull = `${path}/${fileOrFolder}`
         const stat = await fs.lstat(pathFull)
         const isDir = stat.isDirectory()
         const isSymlink = stat.isSymbolicLink()

         if ( isDir || isSymlink )
             directories.push(fileOrFolder)
     }

     return directories
 }
```



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


## 

```javascript                                           
await fs.writeFile('filename.txt', 'test');
```


<br><br>

## write file to json
```javascript                                           
await fs.writeJson('filename.txt', jsonArray)
```



<br><br>

## write file, create directories if not exist and append to file
```javascript                                           
const os = require('os');
const fs = require('fs-extra');

const file = 'logfile.txt';
const options = {flag: 'a'};

async function writeToFile(text) {
  await fs.outputFile(file, `${text}${os.EOL}`, options);
}

writeToFile('First line');
writeToFile('Second line');
writeToFile('Third line');
writeToFile('Fourth line');
writeToFile('Fifth line');
```


</details>

















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Assert Methods (https://www.w3schools.com/nodejs/ref_assert.asp)

<br><br>


## .assert() (- https://www.w3schools.com/nodejs/met_assert.asp)
- The assert() method tests if a given expression is true or not. If the expression evaluates to 0, or false, an assertion failure is being caused, and the program is terminated. The assert() method is an alias of the assert.ok() method.

<details><summary>Click to expand..</summary>

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


</details>



































































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

<details><summary>Click to expand..</summary>



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



</details>










































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Remote Debugging (https://nodejs.org/en/docs/guides/debugging-getting-started/)
- Allows you to debug your node.js code via Remote on your Browser

<details><summary>Click to expand..</summary>

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


</details>






























































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# DNS

<details><summary>Click to expand..</summary>

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

</details>

































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Circular Object

<details><summary>Click to expand..</summary>

## Convert Circular Object to string
```
const axios = require('axios');

axios.get('https://example.com/api/data')
    .then(response => {
        const jsonString = JSON.stringify(response.data, getCircularReplacer());
        console.log(jsonString);
    })
    .catch(error => {
        console.error('Error:', error);
    });

// Funktion zum Ersetzen von zirkulären Referenzen
function getCircularReplacer() {
    const seen = new WeakSet();
    return (key, value) => {
        if (typeof value === 'object' && value !== null) {
            if (seen.has(value)) {
                return;
            }
            seen.add(value);
        }
        return value;
    };
}
```



</details>








































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# stream

<details><summary>Click to expand..</summary>

<br><br>

## create stream
```javascript
const readStream = fs.createReadStream('data.csv')
const writeStream = fs.createWriteStream("outpuData.json")
const stream = readStream.pipe(writeStream)
```

</details>


















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# buffer

<details><summary>Click to expand..</summary>

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

## Get size of string
```javascript
// Method #1
Buffer.from('Test..').length

// Method #2
Buffer.byteLength(string, 'utf8')
```

<br><br>

## Compare Buffer
```javascript
/**
 * Creates a FormData object with the provided file and data.
 *
 * @param {Object} file - The file object containing the fieldname, originalname, and buffer.
 * @param {Object} data - The additional data to be appended to the FormData object.
 * @returns {FormData} The FormData object with the file and data appended.
 */
const _createFormData = (file, data = []) => {
    if (!file) {
        throw new ValidationError('_createFormData() - file')
    }

    const { fieldname, originalname, buffer } = file

    const formData = new FormData()
    formData.append(fieldname, buffer, originalname)

    for (const key in data) {
        formData.append(key, data[key])
    }

    return formData
}


describe('_createFormData()', () => {
    let form, formData

    const imageName = 'file'
    const path = `${__dirname}/img.png`

    before(async () => {
        const imageBuffer = await fs.readFile(path)
        
        form = {
            fieldname: imageName,
            buffer: imageBuffer,
            originalname: imageName
        }

        formData = new FormData()
        formData.append(imageName, imageBuffer, imageName)
        
        for (const key in doc_DatasourceIdWithoutHandlebarsFormData.data) {
            formData.append(key, doc_DatasourceIdWithoutHandlebarsFormData.data[key])
        }
    })

    it.only('should generate form data', async () => {
        const res = _createFormData(form, doc_DatasourceIdWithoutHandlebarsFormData.data)

        const buffer1 = res.getBuffer().toString().replaceAll(res.getBoundary(), '')
        const buffer2 = formData.getBuffer().toString().replaceAll(formData.getBoundary(), '')
        expect(buffer1).to.be.equal(buffer2)
    })
})

```


</details>


























































































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Scripts



## Kill port
```typescript
/**
 * 🚫 Kill process on port 3000
 * @async
 * @function killPort
 * @description Kills any process running on port 3000
 * @param {number} port - Port number to kill process on
 * @returns {Promise<void>}
 */
const killPort = async(port: number): Promise<void> => {
    return new Promise((resolve, reject) => {
        const checkCommand: string = process.platform === 'win32'
            ? `netstat -ano | findstr :${port}`
            : `fuser ${port}/tcp 2>/dev/null`

        exec(checkCommand, (error: ExecException | null, stdout: string, stderr: string) => {
            if (error) {
                console.log(`No process running on port ${port}`)
                resolve()
                return
            }

            const pid: string | undefined = process.platform === 'win32'
                ? stdout.trim().split(/\s+/).pop()
                : stdout.trim().split(/\s+/)[0]

            if (!pid) {
                console.log(`No process running on port ${port}`)
                resolve()
                return
            }

            console.log(`Found process ${pid} on port ${port}`)
            const killCommand: string = process.platform === 'win32'
                ? `taskkill /F /PID ${pid}`
                : `kill -9 ${pid}`

            exec(killCommand, (error: ExecException | null) => {
                if (error) {
                    console.error(`Error killing process on port ${port}:`, error)
                    reject(error)
                    return
                }
                console.log(`Killed process on port ${port}`)
                resolve()
            })
        })
    })
}

```



<br><br>
<br><br>

## Convert csv to json (works with large files)
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






























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# eslint
- **Make sure to restart vs code after installing and configure everything. In most cases it will not detected without restart**

<br><br>
<br><br>

## next.js
- Just install the project and eslint will be setup by default. Then use the the config file from below without extends



<br><br>
<br><br>

## Typescript
- https://typescript-eslint.io/getting-started
```shell
npm install --save-dev eslint @eslint/js @types/eslint__js typescript typescript-eslint
```
- eslint.config.mjs
```javascript
// @ts-check

import eslint from '@eslint/js'
import tseslint from 'typescript-eslint'

export default tseslint.config(
    {
        ...eslint.configs.recommended,
        rules: {
            ...eslint.configs.recommended.rules,
            // Hier fügst du deine neue Regel hinzu
            'arrow-parens': ['error', 'as-needed'],
            'no-var': 1,
            'no-eval': 'error',
            indent: ['error', 4],
            quotes: ['error', 'single'],
            'no-console': 'off',
            'space-before-function-paren': ['error', 'never'],
            'padded-blocks': ['error', 'never'],

            'prefer-arrow-callback': [0, {
                allowNamedFunctions: true
            }],

            'func-names': ['error', 'never'],

            'no-use-before-define': ['error', {
                functions: true,
                classes: true
            }],

            'max-len': ['error', 120],
            'object-curly-spacing': 0,
            'comma-dangle': ['error', 'never'],
            semi: [2, 'never'],
            'new-cap': 0,
            'one-var': 0,
            'guard-for-in': 0
        }
    },
    ...tseslint.configs.recommended
)
```






<br><br>
<br><br>


## ESM
- **Since eslint 9.0 there is no .eslintrc.json file anymore**
  - https://eslint.org/docs/latest/use/configure/migration-guide
  ```shell
  npx @eslint/migrate-config .eslintrc.json
  ```
    
```shell
npm install --save-dev eslint
npm init @eslint/config

# lint all files
# npx eslint .
```

- eslint.config.mjs
```javascript
import path from 'node:path'
import { fileURLToPath } from 'node:url'
import js from '@eslint/js'
import { FlatCompat } from '@eslint/eslintrc'

const __filename = fileURLToPath(import.meta.url)
const __dirname = path.dirname(__filename)
const compat = new FlatCompat({
    baseDirectory: __dirname,
    recommendedConfig: js.configs.recommended,
    allConfig: js.configs.all
})

export default [...compat.extends('eslint:recommended'), {
    rules: {
        'arrow-parens': ['error', 'as-needed'],
        'no-var': 1,
        'no-eval': 'error',
        indent: ['error', 4],
        quotes: ['error', 'single'],
        'no-console': 'off',
        'space-before-function-paren': ['error', 'never'],
        'padded-blocks': ['error', 'never'],

        'prefer-arrow-callback': [0, {
            allowNamedFunctions: true
        }],

        'func-names': ['error', 'never'],

        'no-use-before-define': ['error', {
            functions: true,
            classes: true
        }],

        'max-len': ['error', 120],
        'object-curly-spacing': 0,
        'comma-dangle': ['error', 'never'],
        semi: [2, 'never'],
        'new-cap': 0,
        'one-var': 0,
        'guard-for-in': 0
    }
}]
```








































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


## Import JSON file with ESM
```javascript
// Use object desctructuring
const { propertyName } = (await import("./package.json", { assert: { type: "json" }})).default

// An import assertion in a static import
import jsonObj from './package.json' assert { type: 'json' }

// An import assertion in a dynamic import
const { default: jsonObj } = await import("./package.json", { assert: { type: "json" }})

console.log(jsonObj)
```






















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Util


<br><br>

## promisify()
- https://www.geeksforgeeks.org/node-js-util-promisify-method/
- `util.promisify()` in Node.js converts callback-based methods to promise-based, aiding in managing asynchronous code more cleanly. This alleviates callback nesting issues, enhancing code readability, and simplifying asynchronous operations through promise chaining.
```javascript
// Node.js program to illustrate 
// util.promisify() methods 

// Importing Utilities module 
const util = require('util')

// Importing File System module 
const fs = require('fs')

// Use promisify to convert callback 
// based method fs.readdir to 
// promise based method 
const readdir =
    util.promisify(fs.readdir)

readdir('process.cwd()')
    .then(files => {
        console.log(files)
    })
    .catch(err => {
        console.log(err)
    }) 
```


























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Utils

## Trace
- https://www.npmjs.com/package/@opentelemetry/api


<br><br>

## CLI

### Interfaces
- https://www.npmjs.com/package/listr2

### progress
- https://www.npmjs.com/package/cli-progress

### spinners
- https://www.npmjs.com/package/ora


### Design
- [ink](https://www.npmjs.com/package/ink)





<br><br>


## Logging

### Design
- https://www.npmjs.com/package/boxen
