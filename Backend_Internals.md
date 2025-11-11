# Backend Internals Notes

## Summary - Node.js Fundamentals

### Runtime Environments
- **Node.js**: A runtime environment for JavaScript built with C++ and JavaScript
- **Alternatives**: Bun and Deno are similar runtime environments
- **Bun**: Faster than Node.js and Deno; acts as a drop-in replacement for Node.js with the same syntax

### Getting Started with Node.js
- **First Command**: `npm init` - Initializes a new Node.js project and creates a package.json file

### Command Line Interface (CLI)
- **CLI**: Command Line Interface - a text-based interface used to interact with software and the operating system by typing commands
- Allows developers to execute Node.js scripts, manage packages, and perform various development tasks through terminal commands

### Wrapper Functions
- **Wrapper Function**: Node.js wraps module code in a function before execution
- This provides module-scoped variables and maintains isolation between modules
- Format: `(function(exports, require, module, __filename, __dirname) { /* module code */ })`

### Special Variables in Node.js

#### `__filename`
- Contains the absolute path of the current file being executed
- Example: `C:\Users\project\app.js`

#### `__dirname`
- Contains the absolute path of the directory containing the current file
- Example: `C:\Users\project`

### File System (fs) Module
- **fs module**: A built-in Node.js module for interacting with the file system
- Provides methods to read, write, delete, and manipulate files and directories
- Can be used synchronously or asynchronously
- Example operations: reading files, writing files, creating directories, deleting files

### Exports in Node.js

#### Named Exports
```javascript
// Method 1: Individual exports
exports.add = function(a, b) {
    return a + b;
}

exports.subtract = function(a, b) {
    return a - b;
}
```

#### Default Exports (module.exports)
```javascript
// Method 2: Export multiple functions as an object
module.exports = {
    add: function(a, b) {
        return a + b;
    },
    sub: function(a, b) {
        return a - b;
    }
}
```

**Key Differences**:
- `exports` is used for named exports (adding properties to the exports object)
- `module.exports` is used when you want to export a single entity or completely replace the exports object
- `module.exports` takes precedence over `exports`

### Require in Node.js

#### Importing Built-in Modules
```javascript
const fs = require('fs');  // Import Node.js built-in module
```

#### Importing Local Modules
```javascript
const math = require('./math');    // Import from same directory
const math = require('../math');   // Import from parent directory
```

### Installing and Using Express

#### Installation Steps
1. Initialize Node.js project: `npm init`
2. Install Express: `npm i express`

```javascript
const express = require('express');
const app = express();
```

### Semantic Versioning (SemVer)

**Format**: `MAJOR.MINOR.PATCH`

Example: `3.4.1`
- **Major Version (3)**: Breaking changes - incompatible API changes
- **Minor Version (4)**: New features - backward-compatible functionality
- **Patch Version (1)**: Bug fixes - backward-compatible bug fixes

### Nodemon

**Nodemon**: A development tool that automatically restarts your Node.js application when file changes are detected
- Eliminates the need to manually stop and restart the server during development
- Installation: `npm install -g nodemon` or `npm install --save-dev nodemon`
- Usage: `nodemon app.js` instead of `node app.js`

### Caret Symbol (^) in package.json

**Caret (^)**: Allows updates to minor and patch versions, but not major versions

Examples:
- `"express": "^4.17.1"` - Will update to any version from 4.17.1 up to (but not including) 5.0.0
- Allows: 4.17.2, 4.18.0, 4.99.99
- Blocks: 5.0.0 or higher

**Tilde (~)**: Alternative symbol that only allows patch updates
- `"express": "~4.17.1"` - Will update to any version from 4.17.1 up to (but not including) 4.18.0

### HTTP Module

#### Creating a Server
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World');
});

server.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

**http.createServer()**: Method that creates an HTTP server instance
- Takes a callback function as an argument
- Callback is executed for every incoming request

### Callback Functions

**Callback Function**: A function passed as an argument to another function, which is then executed later (often after an asynchronous operation completes)

```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback('Data received');
    }, 1000);
}

fetchData((data) => {
    console.log(data);  // Executed after 1 second
});
```

### Promises in Async Programming

**Promise**: An object representing the eventual completion (or failure) of an asynchronous operation

**Three States**:
1. **Pending**: Initial state, operation not yet completed
2. **Fulfilled**: Operation completed successfully
3. **Rejected**: Operation failed

```javascript
// Creating a Promise
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve('Operation successful');
        } else {
            reject('Operation failed');
        }
    }, 1000);
});

// Using a Promise
myPromise
    .then((result) => {
        console.log(result);  // Handle success
    })
    .catch((error) => {
        console.log(error);   // Handle error
    });
```

**Async/Await**: Modern syntax for working with promises
```javascript
async function getData() {
    try {
        const result = await myPromise;
        console.log(result);
    } catch (error) {
        console.log(error);
    }
}
```

---

*Last Updated: November 11, 2025*
