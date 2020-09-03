# Node.js Cheat Sheet
Node.js Cheat Sheet with the most needed stuff..


<br />
<br />
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

## set latest version of dependencies
```bash
/* method 1*/
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
```

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />
