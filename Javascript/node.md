# CommonJS (CJS) and ESM - the Node.js module system
cjs - older , using require and module.exports
jsm - modern , uses import export.
For using esm system - for files executed by Node and not by React Native bundler (Metro) + JS engine (Hermes/JSC).
1.  type : module in package.json -  
    or
2.  .mjs extension
    else will get syntax error on import/export.