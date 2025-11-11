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

---

*Last Updated: November 11, 2025*
