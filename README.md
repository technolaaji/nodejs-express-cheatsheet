# Nodejs and Express Cheatsheet



nodejs works on a single thread, non blocking I/O and it handles it in a loop



## Nodejs is best to use:

- Rest API

- Microservices

- Realtime applications

- CRUD applications (blog/shopping cart)

- Tools and Utilities (command line tools as an example)



there are core module like:

- path

- filesystem (fs)

- http



to use them, you will call them in a variable (mostly they will be constants)

```javascript
const path = require('path');
```

you can also import functions that are exported to other javascript files to use them

```javascript
const myfile = require('./myfile');
```



## Starting a Nodejs project

you will start by creating a ```package.json``` file using:

```javascript
npm init 

npm init -y //this won't ask you questions to fill for the package.json file
```



if you are using a specific project on another device, you can install all of your dependencies by using 

```javascript
npm install
```

there are also dev dependencies, these dependencies only work on development enviroment not on production



to install a dependency as a dev dependency

```javascript
npm install -D nodemon
```

if you want to use elements inside of another file, you can export it

```javascript
const person = {
    name: 'John Doe',
    age: 30
}
module.exports = person;
```

then you can import it in another file

```javascript
const person = require('./person');
```

you can also export classes and functions as well

```javascript
class Person {
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    greeting(){
        console.log(`My name is ${this.name} and I am ${this.age}`);
    }
}
module.exports = Person;
```

then you can import it and use it

```javascript
const Person = require('./person');
const person1 = new Person('John Doe', 30);

person1.greeting(); // My name is John Doe and I am 30
```

keep in mind that nodejs doesn't support ES6 importing

```javascript
import Person from './person'
```

you have to use BabelJS or Typescript and see what is backwards compatible to ES5

## Path Module

```javascript
const path = require('path');
```

to get the name of the file (base filename)

```javascript
path.basename(__filename) //path.js
```

to get the directory name

```javascript
path.dirname(__filename) // /users/myPc/nodeFile/pathFile
```

to get the file extension

```javascript
path.extname(__filename) // .js
```

to create a path object

```javascript
path.parse(__filename)

// it will generate this object
{
    root: '/',
    dir: '/users/myPc/nodeFile/pathFile',
    ext: '.js',
    name: 'path'
}
```

since it is an object so you can use any part of it



to concatenate paths

```javascript
path.join(__dirname, 'test', 'hello.html')

// it will create a path like this:
// /users/myPc/nodeFile/pathFile/test/hello.html
```



## File system Module (fs)

```javascript
const fs = require('fs');
const path = require('path');
```

to create a folder

```javascript
fs.mkdir(path.join(__dirname, '/test'), {}, function(err){
    if(err) throw err;
    console.log('folder created....');
})
```

keep in mind that this action is asynchronous so other tasks might happen in the background

there is a synchronous version of this command



to create and write to a file

```javascript
fs.writeFile(path.join(__dirname, '/test', 'hello.txt'), 'Hello world!', err => {
    if (err) throw err;
    console.log('File written to.....');
})
```

you can use arrow functions instead of regular functions 



to add to a file (you use it in the callback)

```javascript
fs.appendFile(path.join(__dirname, '/test','hello.txt'), 'I love Nodejs', err => {
    if(err) throw err;
});
```

to read from file

```javascript
fs.readFile(path.join(__dirname, '/test', 'hello,txt'), 'utf8', (err, data) => {
    if(err) throw err;
    console.log(data);
});

// Hello world! I love Nodejs
```

to rename a file

```javascript
fs.rename(path.join(__dirname, '/test', 'hello.txt'), path.join(__dirname, '/test', 'helloworld.txt'), err => {
    if(err) throw err;
    console.log('File renamed......');
});
```



## Operating System Module (os)

```javascript
const os = require('os');
```

to get the platform

```javascript
os.platform()

// darwin for macOS and win32 for windows
```

to get CPU architecture

```javascript
os.arch()
// x64
```

to get CPU core info

```javascript
os.cpu()

// it will return a big object with how many cores and much more
```

to get free available memory

```javascript
os.freemem()

// 122388480
```

to get total memory

```javascript
os.totalmem()

//34359738368
```

to get the home directory

```javascript
os.homedir()

// /users/myPc
```

to get uptime (time the system has been up)

```javascript
os.uptime()

// 1305116 (in seconds)
```



## Url Module (url)

```javascript
const url = require('url');
```

to create a url

```javascript
const myUrl = new url("http://myepicwebsite.com/hello.html/?id=10");
```

to serialize the url

```javascript
myUrl.href
myUrl.toString()

// http://myepicwebsite.com/hello.html/?id=10
```

to get the host

```javascript
myUrl.host

//myepicwebsite.com
```

to get the hostname (same as host but minus the port)

```javascript
myUrl.hostname

//myepicwebsite.com
```

to get the pathname

```javascript
myUrl.pathname

// /hello.html
```

to serialize a query

```javascript
myUrl.search

// ?id=10
```

generate a params object (basically queries in an array)

```javascript
myUrl.searchParams

// { 'id' => '10', 'status' => 'active'}
```

you can add to that param by appending to it

```javascript
myUrl.searchParams.append('abc', '123');

// { 'id' => '10', 'status' => 'active', 'abc' => '123'}
```

you can loop through the params

```javascript
myUrl.searchParams.forEach((value,name) => 
    console.log(`${name}: ${value}`);
);
```



## Event Emitter Module

```javascript
const EventEmitter = require('events');
```

first create a class 

```javascript
class MyEmitter extends EventEmitter {}
```

initialize an object

```javascript
const theEmitter = new MyEmitter();
```

create an event listner

```javascript
theEmitter.on('event', () => console.log('Event!'));
```

initialize an event

```javascript
theEmitter.emit('event');

// Event!
```



# Express

```javascript
const express = require('express');
```

initialize express

```javascript
const app = express();
```

create your endpoint/ route handler

```javascript
app.get('/', function(req,res){
    res.send('Hello world!');
});
```

listen to a port

```javascript
app.listen(5000);
```



basic route handling

```javascript
app.get('/', function(req,res){
    // fetch from database
    // load pages
    // return JSON
    // full access to request and response
})
```



you can set up a port variable that takes either the server's port or a port of your choice if server doesn't have a port defined to it

```javascript
const PORT = process.env.PORT || 5000;

app.listen(PORT);
```

you can parse HTML in a response

```javascript
app.get('/', (req,res) => {
    res.send('<h1> Hello world </h1>');
});
```

to send a file

```javascript
const express = require('express');
const path = require('path');

const app = express();

app.get('/', (req,res) => {
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
});
```

in express, you can set up a static folder to host static content

```javascript
app.use(express.static(path.join(__dirname,'public')));
```



to return json

```javascript
const members = {
    // a object that contains users with id/name/email
    // id: 1,
    // name: 'John Doe',
    // email: 'john@doe.com'
}
app.get('/api/members',(req,res) => {
    res.json(members);
});
```

it will stringify the object in json format



creating middleware (code that works behind the scene)

```javascript
const logger = (req,res,next) => {
    console.log(`${req.protocol}://${req.get('host')}${req.originalUrl}`);
 next();
}
```

to use this middleware

```javascript
app.use(logger);
```

to use url parameters

```javascript
app.get('/api/members/:id', (req,res)=> {
    res.send(req.params.id);
});
```

to get a single member from that member's object we created before

```javascript
app.get('/api/members/:id', (req,res) => {
    const found = members.some(member => member.id === parseInt(req.params.id));
    
    if(found){
        res.json(member.filter(member => {
            member.id === parseInt(req.params.id)
        }))
    }
    else {
        res.status(400).json({msg: "member not found"});
    }
});
```

the some function returns a true/false based on the condition if it is satisfied

the filter will return an object with the elements that satisfy its condition

you parse the request parameters to integer because by default it is a string



if not found return an error json with status of 400 (bad request), majority of the requests are send with status 200 (good request) so just to make sure that it is an error you add the 400 status



express routing (you don't have to include every single route in app.js so you can use a router)



assume you created a folder called routes, inside of it a folder called api then a javascript file called member.js



so it will look like this  

routes -> api -> members.js



inside members.js

```javascript
const express = require('express');
const router = express.Router();
const members = { 
     // that object we define previously
};

router.get('/'(req,res)=> {
    res.json(members);
});

module.exports = router;
```

to use this router, in app.js

```javascript
app.use('/api/members', require('./routes/api/members'));
```

if you call now /api/members with a get request, it will give you the members



example if a post request

```javascript
router.post('/', (req,res) => {
    res.send(req.body);    
});
```

the body won't work because it is not parsed

to parse it, you add these lines to app.js

```javascript
app.use(express.json());
app.use(express.urlencoded({extended: false})); // for url encoding
```



create a member example:

```javascript
router.post('/', (req,res) => {
    const newMember = {
        id: req.body.id, // or you can use a package like uuid
        name: req.body.name,
        email: req.body.email,
        status: 'active'        
    };
    if(!newMember.name || !newMember.email){
        return res.status(400).json({msg: "provide email or name"});
    }
    members.push(newMember);
    res.json(members);
});
```



update a member

```javascript
router.put('/:id', (req,res) => {
    const found = members.some(member => member.id === parseInt(req.params.id));
    
   if(found){
       const updateMember = req.body;
       members.forEach(member => {
           if(member.id === parseInt(req.params.id)){
               member.name = updateMember.name ? updateMember.name : member.name;
               member.email = updateMember.email ? updateMember.email : member.email;
               res.json({msg: "updated"});
           }
           else {
               req.status(400).json({msg: "didn't update, user not found"});
           }
       })
   }
});
```

delete a member

```javascript
router.delete('/:id', (req,res) => {
    const found = members.some(member => member.id === parseInt(req.params.id));
    
    if(found){
        members = members.filter(member => member.id !== parseInt(req.params.id));
        res.json({msg: "member deleted"});
    }
    else {
        res.status(400).json({msg: "cannot delete, member not found"});
    }
});
```
