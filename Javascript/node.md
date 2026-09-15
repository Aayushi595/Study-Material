# CommonJS (CJS) and ESM - the Node.js module system
cjs - older , using require and module.exports
jsm - modern , uses import export.
For using esm system - for files executed by Node and not by React Native bundler (Metro) + JS engine (Hermes/JSC).
1.  type : module in package.json -  
    or
2.  .mjs extension
    else will get syntax error on import/export.


# Node.js Jacvascript and Execution 

Node.js : Node.js is a runtime environment that lets you execute JavaScript outside a browser.

libuv is a C library that provides Node.js with its event loop and asynchronous I/O infrastructure.

Worker thread : additional thread that can run JavaScript separately from Node.js's main JavaScript thread. To perform CPU-heavy JavaScript work without blocking Node's main event loop. Equivalent to web workers in browser.  

- Things own by whom : 
    Node.js
    ├── JavaScript engine → V8
    ├── Node APIs
    │    ├── fs
    │    ├── http
    │    ├── crypto
    │    ├── streams
    │    └── etc.
    ├── fetch
    ├── setTimeout
    ├── EventEmitter
    ├── Event Loop → libuv
    ├── Microtask queues
    └── worker_threads 

- service worker equivalent in Node .
   Intercept HTTP requests   -   Middleware
   Handle Http Request       - http / Express / Fastify
   Cache responses           -  Redis / in-memory cache / filesystem
   Push notifications        - Server-side push libraries/services
   Run independently of a webpage    -  Node server/process
   