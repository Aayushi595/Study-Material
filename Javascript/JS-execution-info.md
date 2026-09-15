
# Javascript
1. interpreted - compilation and conversion to machine code happens at runtime. (no additional bytecode file)
2. dynamically typed - no type declaration needed =>  no type checking required prior => no compilation prior to script run.
3. synchronous single threaded.
4. loosely typed/weakly typed  - can reassign any type of value to a variable. Type need not be to be defined when declaring a variable. JavaScript tries to automatically convert types when they don’t match - Implicit conversion / type coersion.
Platforms -  browser - in-browser js - v8/spidermonkey
             server - Node.js - v8 engine
             mobile - React Native - Hermes
             basically everything that have Javascript Engine.
Unique - Full integrated with HTML/CSS. Can run in browser, server, mobile.

# In-browser js vs Node.js vs bundle.js (bundled by metro)
In-browser js : Has access to window, document, DOM APIs, Read/write local files freely , cannot access OS. When JS runs in a browser, Code comes from random websites. A malicious site could steal your files, Delete your data, Install malware.
Node.js : No access to DOM, Have access to File system (fs), OS. Node.js can read/write files because it runs on a trusted machine (server/local), not inside a public browser sandbox.
Bundle.js (Mobile) : Talks to native mobile code (Android/iOS) via bridge.

# In-browser JS - in Html doc script
 script tags - 
   1. <script>alert('hello')</script>
   2. <script src="/path/to/script.js"></script> - external script
       <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js"></script> - third party.
   <script src="file.js">
      alert(1); // the content is ignored, because src is set
      </script>

   The benefit of a separate file is that the browser will download it and store it in its cache. Other pages that reference the same script will take it from the cache instead of downloading it, so the file is actually downloaded only once.

   # script tag
  By default, <script> is treated as a classic script(browser wali js).That means no import or export statements are allowed in the file.
  If your app.js uses import, you must tell the browser (or Parcel) that it’s a classic JavaScript(js of browser) module by writing: <script type="module" src="./app.js"></script>

  1. Browser does not allow JS to read data of another website/website opened in another tab under SOP (same origin policy). why ?
    Imagine this:
    You open your bank → https://mybank.com
    In another tab, you open a random site → https://evil.com ,  now evil.com is able to see my bank balance. 

    Browser allows opening a website using js  - let w = window.open('https://www.programiz.com/javascript/online-compiler/'); 
    But not accessing its dom or reading data : 
          - console.log(w.document.body ) -  VM196:4 Uncaught SecurityError: Failed to read a named property 'document' from 'Window': Blocked a frame with origin "chrome://new-tab-page" from accessing a cross-origin frame.
          - fetch('https://www.programiz.com/javascript/online-compiler/'); - VM220:1 Fetch API cannot load https://www.programiz.com/javascript/online-compiler/. Refused to connect because it violates the document's Content Security Policy.
                    (anonymous) @ VM220:1Understand this error
                    Promise {<rejected>: TypeError: Failed to fetch
                      at <anonymous>:1:1}


  2. JS cannot access OS / local files freely. Why ?
  Any website could steal your files. It allows manually selecting file using : <input type="file" />

# Modules

# Built-in APIS/ function
ECMAScript = Modern JS Engines (Any platform using Modern JS Engines can execute Ecmascript things.)

- Web APIs (browser APIs - provided by browser)
DOM API - document, window,Element, Node, HTMLElement,document.querySelector()
Network API - XMLHttpRequest, EventSource, WebSocket
Storage APIs - local Storage, Session Storage
Event APIS - addeventListener, removeEventListener
Default events listened by browser - click, hover, mousemove.
Geolocation API - history, location, navigator
Rendering API - 
Media API

- JS Engine based built-in APIs / functions  - browser, node.
Timer API - setTimeout, clearTimeout, setInterval
import/export , async/await 

- Node Runtime APIs : process, Buffer, path, http, os, fs, require (cjs), __dirname, __filename, module. This will give Reference Error in browser and RN (Hermes).

# JS in different Engines.
1. Browsers (SpiderMonkey, V8)
  Browser setup contains :  
    - EventLoop
    - DOM
    - redering Engine
    - Javascript Engine

2. Node.js (V8)
   node process :
     - event loop
     - file system
     - http
     - timers
     - js engine

# JS Code Execution.
1. Initial Parsing - JS Scans the entire script to build an Abstract Syntax Tree.
2. GEC created.
3. Execution context two phases: 
   Memory / creation phase - All the toplevel variables are functions are assigned memory (local memory).
   Code Execution - variables are asigned value, Function invocation creates a new execution context of itself. The function param is also assigned memory in the memory allocation phase of this function. In execution phase, as soon as reurn is entountered, function return the control to where it was invoked.

   Execution Context is managed using call stack. GEC at bottom of stack. At the end, GEC is also poped.

  - Memory Creation Phase and Hoisting.

# Error, exception, warning (Browser, Node.js)
Error = object, new Error("Boom");
Exception = event of throwing that error, throw new Error(...);
The exception travels up the call stack looking for try-catch. If not found ,  Uncaught Exception. Uncaught Exception causes code break and Process Execution stops. (Thumb rule)
        Exception thrown
        ↓
        No try/catch
        ↓
        Uncaught Exception
        ↓
        Process exits (complete stack is emptied and the complete script stops.)

1. In parsing phase : Syntax Error -  stops entire code block from running.
2.  Runtime : 
      1. Type Error - the operation being performed on a varibale is incompatible/incorrect.
      2. Reference Error - try to access a variable or function that does not exist in current scope. 
      3. Unhandled promise Rejection - promise rejection not handled using catch. 
          Results : 
          1. Synchronous 
            Promise.reject("Failed");     
            console.log("A");        //A will be printed then unhandled promise rejection is thrown.
            Because promise rejection happens asynchronously. The rejection is reported later by the microtask queue. The current call stack has already finished.
          
          2. Inside a normal function
            function fn(){
                Promise.reject('1')
                console.log("djhvjfv")          //djhvjfv then hellooo then unhandled promise rejection.
            }
            fn()
            console.log("hellooo")

          3. Inside an async function with await 
            async function fn(){
              await Promise.Reject("63")
              console.log('123)
            }
            fn()
            console.log("hellooo")         //hellooo then unhandled promise rejection.
            
            This can be handled using try-catch or stored the result in a variable . eg :
            const var = Promise.reject("453")
          
        4. File System and Network Error - Generally promise based apis so error works according to it.
        5. Module Resolution Error - Error but continues.
        6. Logical Errors - No error,  but unexpected results.

        # Error inside eventListener
        In browser - only the function handling the event is removed after error, when the event is triggered. 
        In Node.js - process ends, code after the event emmitter trigger is not executrd.


# let, var, const - Scope and Hoisting, Lexical Env.
Scope of a variable - the block/part of code where we can access a variable.
Scope is created by - funtion, global scope (and not object), block (object does not create a new scope).
Scopes for this - funtion, global scope (and not object).

let, const and var declarations are hoisted. 
let & const are hoisted but in TDZ. If we access a before initilization statement, it gives reference error. 
 Ex :  a exists in memory
       a is uninitialized

var is declared with undefined assigned. No reference error even if we try accessing it before initialization.
let - block scope . Can be reassigned and cannot be redeclared on same scope.
const - block scope. needed initialization when declared. Cannot be declared or reasigned in same scope.
var - function scope. Can be redeclared and reasigned in same scope. 
Redeclaring var as const / let, simply redeclaring a let, simply redeclaring a const : Syntax error : Identifier 'x' has already been declared


Lexical Env. : 
Closure : Lexical Env for function. Function with its lexical env (parent's env)

# this in different scopes.
this - this is a special runtime keyword in JavaScript that represents the execution context.
global scope - this = window. (non scrict mode)
block   - does not create a new this value.
function - this is a special keyword that refers to the object that is executing the current function, how the function is called/who called the function.
function declared in global scope , has value of this = window (in non-scrict mode). Normal function has the capability to chnage the value of this based on how it is called, using call, apply, bind, while arrow function don't.
arrow function - does not create this binding of its own. value of this inside arrow function = value of this in lexical scope where the arrow function is defined.

# Primitive types, datastructures, operations and conversions (explicit / implicit (type coersion)) in JS, memory model.

- Primitive types (Value Types) - string, number, bigint, null, undefined, boolean, symbol.
  The idea of primitive type is to design a single, indivisible value. Thus, Primitive types are immutable. (original value is never changed)

  Primitive types are also called value types because they are copied by value in memory.
    ex : let a = 10; let b = a; b = 20;   In Memory : a -> 10 b-> 20

  Need : 1. Performance (Memory Sharing) : If a program uses a string or other value 1000 times, then 1000 new copies will be created, if the program also modifies the original value.
         2. Safety - if a value is passed as param to function, then we to think that the function might modify its value.

  
  1. number - Number.MAX_SAFE_INTEGER = 2^53 - 1 . Beyond this, JavaScript starts losing precision.
    Ex :  
      console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
      console.log(Number.MAX_SAFE_INTEGER + 1); // 9007199254740992
      console.log(Number.MAX_SAFE_INTEGER + 2); // 9007199254740992 ❌

  2. bigint - For numbers bigger than Number.MAX_SAFE_INTEGER. 
    - const big = 123456789012345678901234567890n; tells js that this is bogint
    - const big = BigInt("123456789012345678901234567890");  //other way of creating
    - BigInt does not have a fixed maximum or minimum value defined by the JavaScript language. Its limit = available size of your memory. 
    - Don't support decimal
    - Math.sqrt(16n);   // typeerror.
    - Cannot mix BigInt and other types directly. explicit conversion needed. else Typeerror.
    - Comparisons work.

    - operations with two bigint
      const a = 100n;
      const b = 30n;

      console.log(a + b); // 130n
      console.log(a - b); // 70n
      console.log(a * b); // 3000n
      console.log(a / b); // 3n .   note => this removes the fractional part.

    3. String . 
     Template Literals .  

- Reference Types : Object.
  # Object 
    A container that can store data and behavior (properties and methods). How that data is stored depends on which type of object it is. Objects in JS have different internal structures/syntax depending on what kind of object it is. But posses common properties. they have properties, methods, idendity (two objects with same content are different), stored and passed by reference.

    Objects are called reference (address) types because : JS Copies the address of object.
    Reason : Memory Efficiency. Suppose there is a very big object, and if the same object is copied into different variables. This will consume memory for each new variable.

    Ex : const a = {
      name : 'Aayu'.    //10mb in memory
    }
    const b = a;
    const c = a;    //here a, b, c points to same address at which the object {name: 'Aayu'} is stored. No multiple value in memory.

    Prototype : Every object has an internal hidden property called prototype which is basically an object. This contains some built-in methods on that object. we can access it using Object.getPrototypeOf(obj), obj.__proto__ 
   
    Ex . const arr = new Array(1,2,3)
    Internally it is : [
      properties
      0:1, 1:2, 2:3 
      length : 3
      [[prototype]] //this contains push(),  pop() etc.
    ] 
    Array.prototype === array.__proto___  //displays based on env. 
    Array.prototype.push.  // [Function: push]

    Anthing Other than primitive types in JS are ojects. Reason : they have properties, methods, idendity (two objects with same content are different), stored and passed by reference.
    Think it of like Array is an object containing some standard props and methods which can be shared by all the instances of array.

    1. Array, Map, Set, WeakMap, WeakSet, Number, String, Boolean, Date, class instance, Promise, Error. These are mutable. BigInt, Symbol is not a constructor.
    Examples : 
      const arr = [1,2,3]    // typeof(arr) object, value : [1,2,3]
      const arr = new Arr([1,2,3])  // typeof(arr) object. , value : [[1,2,3]]
      const arr = new Arr(1,2,3)  // typeof(arr) object. , value : [1,2,3]
      const number = new Number(8). // typeof(number) object, value : [Number: 8].   //It is just how Node.js (or the browser console) chooses to display a Number object.
      const string = new String('56') // typeof(string) object, value : [String: '56']
      const bool = new Boolean(2) // typeof(bool) object, value : [Boolean : true]
      const set = new Set([1,2, 3]). // typeof(set) object, value : Set(3) { 1, 2, 3 }
      const map = new Map([[1, 1], [2, 2]]); // typeof(map) object : Map(2) { 1 => 1, 2 => 2 }
      const pr = new Promise((res, rej) => console.log('hello')). // typeof(pr) object , value : Promise { <pending> }
      const er = new Error('fail') // typeof(err) object,  value : ERROR!
        Error: fail
            at Object.<anonymous> (/tmp/3LH6UdOFKr/main.js:2:12)
            at Module._compile (node:internal/modules/cjs/loader:1706:14)

      Mutability: 
        const num = new Number(8);
        num.name = "Age";
        console.log(num);     // [Number : 8]{name : "Age"}

    2. Function.
    const fn = () => {} //typeof(fn) Function. Because JavaScript treats functions differently from other objects because they can be called.
    console.log(fn instanceof Object);  //true
    console.log(fn instanceof Function);  //true
      +--------------------------------+
      | [[Call]]  → executable code    |
      | name: "fn"                     |
      | length: 0                      |
      |--------------------------------|
      | [[Prototype]] ---------------------> Function.prototype
      +--------------------------------+
    
    Object Destructuring : 
    nested : const obj = {
      name : {
        age : 50
      }
    }

    const {name, name : {age}} = obj

  - Memory Model
    Stack - fast memory. stores primitive types and references (addresses)
    Heap - Large memory. Actual Object definitions.

    # Imp
    let yt = 10
    let st = yt  
    st = 30.     // value of yt is still 10 and st is 30 - Because primitive types are copied by value. Note a new copy for st is created.
    const obj = {}
    const obj2 = obj1
    console.log(obj2)   // {} - note this does not log address.
    obj2.name = "Aayu"
    console.log(obj)    // {name:"Aayu"}
    console.log(obj2)   // {name:"Aayu"} Value of obj and obj2 both chnaged because both points to same object in heap, because same address is copied into stack for both variable obj and obj2

    #  Operators	and coercion.
        ==	✅ Yes
        !=	✅ Yes
        ===	❌ No
        !==	❌ No
        +	✅ Sometimes (but not between BigInt and Number)
        -	✅ Sometimes (but not between BigInt and Number)
        *	✅ Sometimes (but not between BigInt and Number)
        /	✅ Sometimes (but not between BigInt and Number)
        <, >, <=, >=	✅ Yes (with special rules for BigInt and Number)

        spread - 
          const args = [1,2,3]
          console.log(...args) // 1,2,3 - Note this is not an array

          function join(...args){
              console.log(args).     //[1,2,3] - it's an array
          }

      Nullish Coalescing operator -
        optional chaining (?.) only checks for null or undefined.
        Empty string is falsy.


Data Structures (Built-in): 
  Array, Map, Set, WeakSet, WeakMap, Objects.
  Arrays are mixed data types unlike in C++.


# Functions in Javascipt
short argument case - function join(a,b,c){console.log(`${a}_${b}_${c}`)} join(1).  //1_undefined_undefined
extra argument - function join(a,b,c){console.log(`${a}_${b}_${c}`)} join(1,2,3,4,5) //1_2_3

- functions are first class functions in JS. 
   passed as argument to another function . (Callback function)
   Get returned from another function.
   Functions that actually either take function as arg , return function - Higher Order functions.

- spreading args
logic 1 : function wrapper (...args){
      console.log(...args)          //1 2
      console.log(args)             // [1, 2]
  }                 

logic 2 : accepting array as arg
  const f1 = (args) => args[0] * args[1]
  f1([1, 2])                              // correct

logic 3 : spreading the args
  function f1(...args){
      console.log(args[0] , args[1])
      return args[0] + args[1]; 
  }
  f1([1, 2])                    // [1,2] undefined                      

- Ways of consuming functions : 
1. Function statement/declaration , Function expression. Function expression is treated as variable. Including Hosisting rules of let, var,const.
2. Normal function and Arrow function.
3. Curried Function
4. IIFE (Immediately Invoked Function Expression) : 
(function () {
  console.log("Hello");
})(); 

(() => {
  console.log("Hello");
})();
5. Function Composition.
when we need to apply a set of functional operations in sequence, where each function takes the result of last as an argument. 
sum -> multiply result with itself (square)
composition function : takes functions as args, return a wrapper function that have args, this wrapper returns composition f2(f1(args))

Ex : f2 takes single arg only.
  const f1 = (a, b) => a + b;
  const f2 = x => x * x;

  function comp (...args){
    return f2(f1(...args))        
  }
  Modern - let comp = (...args) => f2(f1(...args))


  function comp (f1, f2){
    return function wrapper (...args){
      return f2(f1(...args))
    }
  }
  Modern - const compose = (f1, f2) => (...args) => f2(f1(...args));

Generic : function pipe (...fns){              // when we use reduce left -> right
  return function (...args){
    return fns.reduce((a,b) => b(a), args) 
  }
}

Generic : const compose = (...fns) =>
  (...args) =>
    fns.reduceRight((acc, fn, index) =>
      index === fns.length - 1 ? fn(...acc) : fn(acc),      // right to left
    args); 

Note : we cannot spread args in the initial value, because the syntax of reduce will break and it we only take the 1 st element. We need to handle array args in the f1 function/first function.

const f1 = (args) => args[0] * args[1]

# Arrow func vs Normal function
- Arrow function : 
1. Do not create their own binding of this - this is taken from the lexical scope where is the function defined.
value of this inside arrow function = value of this in lexical scope where the arrow function is defined.

2. Value of this Cannot be changed with call, apply, bind

Ex : const obj = {
   name: "aayushi",
   greet:  () => {
      console.log(this.name);
   }
   };
   obj.greet(); undefined. //binding of this not created.

3. Execution - 

4. Cannot use arrow function as a constructor, gives TypeError: Car is not a constructor, when creating instance.
  
5. arrow functions does not have default argument object, which regular function has.
        argument object : contains all the params passed to to function, even if the function definition don't declares any.
        Ex - function sum() {
            console.log(arguments); 
            }
            sum(1, 2, 3);           // [Arguments] { '0': 1, '1': 2, '2': 3 }

6. Easier syntax.

- Normal function - binding of this - where it is called and how the function is called - dynamic.
this can be changed with call,apply,bind.

Ex : const obj = {
   name: "aayushi",
   greet: function () {
      console.log(this.name);
   }
   };
   obj.greet(); // ✅ "aayushi" - binding of this created with obj
   const fn = obj.greet
   fn().  //undefined - Now it is called as fn() (not obj.fn()), so the connection to obj is lost.

# When you should use arrow functions in objects / Why arrow function are designed to use this from its lexical scope where they are defined / Why arrow functions are introduced.
Use arrow functions when you want to inherit this from the parent function, eg. with setTimeout()
Ex :   const user = {
  name: "Ravi",
  greet: function () {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
};
user.greet();  // Ravi - undefined if have used normal function in the setTimeout. This the biggest reason why arrow functions exist.

Before ES6+, developers used
1. const obj = {
  name: "Aayushi",
  greet: function () {
    var self = this;

    setTimeout(function () {
      console.log(self.name);
    }, 1000);
  }
};

2. setTimeout(function () {
   console.log(this.name);
   }.bind(this), 1000);

This combination was so common to use that's why, they invented a function that will not have its own this. 


# Call/Apply/Bind
call / apply / bind are methods on all functions. But they only control this for normal functions.

- Call : Call the function immediately with a specified this, passing arguments one by one.
func.call(thisArg, arg1, arg2, arg3);

Ex : function greet() {
      console.log(this.name);
      }
      const obj = { name: "Aayushi" };
      greet.call(obj);  // Aayushi

Ex : const greet = () => {
   console.log(this.name);
   };
   const obj = { name: "Aayushi" };
   greet.call(obj);  // undefined


- Apply : Call the function immediately with a specified this, but pass arguments as an array.
func.apply(thisArg, [arg1, arg2, arg3]);

   Ex : function sum(a, b) {
   return a + b;
   }
   sum([2, 3]);  // [2, 3] + undefined // ❌ not what you want
   modern : sum.apply(null, [2, 3]); // 5

  Ex : function introduce(age, city) {
     console.log(this.name, age, city);
   }
   const person = { name: "Aayushi" };
   introduce.apply(person, [22, "Jaipur"]);
     
  - Useful with built-in functions (like Math)
      const numbers = [5, 10, 2, 8];
      const max = Math.max.apply(null, numbers);
      console.log(max); // 10

      Modern solution : Math.max(...numbers);

  - Borrow method from other objects.
      const person1 = {
      name: "Alice",
      greet: function(age) {
         console.log(this.name + " is " + age);
      }
      };
      const person2 = { name: "Bob" };
      person1.greet.apply(person2, [25]);

Syntax : args are optional, 1st arg - The value you want this to refer to inside the function.

- Bind : Create a new function with this permanently bound. It does not execute immediately.
Syntax : const newFunc = func.bind(thisArg, arg1, arg2);

EX : function greet() {
  console.log(`Hi, I'm ${this.name}`);
}
const person = {
  name: "Aayushi"
};
const boundGreet = greet.bind(person);
boundGreet();                           // Aayushi


# Built-in function and Polyfills
curry(), debounce(), throttle(), setTimeout(), setTimer(), clearTimer(), call, apply, bind
typeof(pr). : typeof returns strings - string, number, bigint etc.
Number, String, Boolean, Object, Array, Date - normal function and a constructor. 
Ex :   const a = new Number("1")   
       const b = Number("1")  
       console.log(typeof(a));   // object
       console.log(typeof(b));   // number
 
- Debounce 
Debounce delays a function’s execution (that is function will execute after x ms of inactivity (no furthur API Call/ competion of this task eg. after 3ms of user stops typing in search)
and resets the delay if the function is called again within that time, ensuring only the last call is executed after the delay period.
eg. search typing

function debounce(func, wait) {
  let timer;

  return function (...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      func(...args);
    }, wait);
  };
}

- Throttle
Throttle executes a function once per given ms with latest arguments. 

Algo : 
1st call execute and timer start.
Intermediate calls will be cut using isthrottled = true (cooldown), but save args, for latest call.
after given time ends, wrapper execute, cooldown is cancelle isthrottled = false, args are reset.

- Currying is a transformation of functions that translates a function from callable as f(a, b, c) into callable as f(a)(b)(c).
The result of curry(func) is a wrapper function(a).
When it is called like curriedSum(1), the argument is saved in the Lexical Environment, and a new wrapper is returned function(b).
function curry(f) { // curry(f) does the currying transform
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}
function sum(a, b) {
  return a + b;
}
let curriedSum = curry(sum);

- Function composition and pipe()


 
   

# String and Methods
Note - Strings are immutable in JS, there is no method that modifies original strings.

1. string.includes(searchText) (check substring)
2. slice() → extracts a part of a string, takes index. Original string is not affected.
3. split() → Converts a string into an array, takes separator(string), only for strings.
Original string is not affected.
Syntax : string.split(separator, limit) : limit is optional. Limit is maximum number of pieces.
EX : const str = "A-B-C-D-E";
console.log(str.split("-", 3));       //["A", "B", "C"]
console.log(str)   // A-B-C-D-E
4. str.substring(0, 5);
5. str.replace("World", "JS");
6. str.concat("!!!");
7. str.trim();
8. str.toLowerCase();
9. str.toUpperCase();
10. str.repeat(2);

# Array and Array Methods, Polyfills.
1. slice  - Extracts a part of array.
Original array is not affected.
Ex : const args = [1,1]
console.log(args.slice(0, 4)).  // [1,1] , second arg = desired + 1

2. every(x => x!=curry.placeholder). //returns boolean

3. splice . (only for array)
purpose : Add, remove, replace elements in the original array. Hence it modifies original array. 
note - here index argument is index from 0 + 1.
splice returns an array of deleted elements.  

Add element - const arr = [1, 2, 5]; //  you want - [1,2,3,4,5]
const spl = arr.splice(2, 0, 3, 4) // start from index 2, delete 0 elemnt, add 3, 4
console.log(spl)     // []
console.log(arr)     // [1,2,3,4,5]

Delete - const arr = [10, 20, 30, 40];
const spl = arr.spl(2, 1)  // start from index 2 , and leaving index 2 - delete 1 element.
console.log(spl);         // [30]
console.log(arr);        //  [10, 20, 40]

Replace - delete then insert
const arr = [1 , 4 , 3]
const spl = arr.splice(1, 1, 2)
console.log(spl)                   // [4]
console.log(arr)                   // [1,2,3]

4. push - updates original array
5. pop - updates original array
6. sort - updates original array
7. reverse - updates original array

8. Map - polyfill

9. Filter - polyfill
10. Reduce  - polyfill
The result of reduce() is always a single value. reduce an array to a number, string, array, object, map, promise, function.

syntax : array.reduce((accumulator, currentValue, currentIndex, array) => {...}, initialValue) // initialValue of accumulator.
Accumulator and currentValue (iterated value) is used mostly. currentIndex need not be explicitly written. It is bydefault populated by reduce.
each iteration automatically returns updated value of accumulator, which is used for nrxt iteration.

Algo : function myReduce(arr, callback, initialValue) {
  let acc = initialValue;
  for (let i = 0; i < arr.length; i++) {
    acc = callback(acc, arr[i], i, arr);
  }
  return acc;
}

# JS Classes (ES6+) Inheritance and PHP Class Inheritance.
whenever a new instance of class is created, constructor is called.  
Javascript :
class is syntactic sugar over prototypal inheritance.
If a class extends another, the derived constructor cannot use this until super() is called, because this isn’t initialized yet.

Example : class Parent {
constructor(props) {
this.props = props;
}
}

class Child extends Parent {
constructor(props) {
console.log(this.props); // ❌ ReferenceError
super(props); // ✅ initializes this
console.log(this.props); // ✅ works now
}

console.log(this.props) //Invalid-Syntax error
render() {
console.log(props); //ReferenceError: props is not defined
return <Text>Hello {this.props.name}</Text>;  
 }
}

PHP:
Class-based inhetitance
$this is always available; calling parent::\_\_construct(), runs parent logic

Example : class ParentClass {
public function \_\_construct($props) {
$this->props = $props;
}


class ChildClass extends ParentClass {
public function **construct($props) {
echo $this->props ?? "not set"; // ✅ allowed, prints "not set"
parent::**construct($props); // optional: initializes parent
echo $this->props; // ✅ now set
}
}

new ChildClass("Aayushi");

=>Normal function vs Constructor
Doesn’t require new keyword.
Does not create an object automatically. special function meant to create and initialize objects.
Return value is whatever you explicitly return (or undefined if nothing). If you don’t explicitly return an object, it will return this (the new object).

Example Constructor : function Person(name) {
this.name = name;
}

const p = new Person("Aayushi"); // ✅ with new
console.log(p.name); // "Aayushi"

=> Class-Based Component :
parent constructor --> Parent render --> child constructor --> child render --> childComponentDidMOunt --> PrentComponentDidMOunt

Mounting Phase : constructor --> render (with default data for few ms)--> ComponentDidMount (called on initial render only - API Call , setState)
Update Phase : setState triggers this . render (api data) --> componentDidUpdate
UnMounting Phase :
ComponentWillUnMount : When we switch components (pages) in single page application.
Usecase : to clear async functions' and execution, that continues even when the component or page is switched. (eg. setInterval, setTimeout), this remains hangs in the browser. If the page conataining setInterval is switched 3 times, three new setInterval is starts and run contibuosly.








# How do var le and kant differ in scope and hoisting behavior and when would you choose each in production JavaScript?
# Calling out var as function scoped and let us block scoped gets to the core and the collision slash maintainability point is why most teams avoid var.
# How would you explain the temporal dead zone for let CONST and what kind of bug does it prevent?
# Switching gears, explain how Javascript's event loop prioritizes microtasks vs macros and what order you'd expect for promise, then Q microtask and settimeout 0.
# In your own words, what's the execution order between synchronous code promise dot then Q microtask and set timeout 0 in a typical browser event loop?
# In React, what's the order of life cycle phases from initial render through commit and where do use effect and use layout effect run relative to Dom updates in browser pane?
# In React 18 with strict mode enabled in development, why can use effect run twice on mount? And what lifecycle related bugs is React trying to surface?
# How would you structure an effect that subscribes to an external store, for example websocket? So it's correct under strict mode, double invocation and avoids duplicate subscriptions.
# In concurrent rendering, a render can be started and then abandoned. What should you avoid doing during the render phase, including use memo initializers to prevent lifecycle related bugs?
# When a parent rerenders, under what conditions will a memoized child react memo still re render and how does that relate to prop identity and lifecycle slash performance?
# Under what conditions will a react dot memo child still re render when its parent re renders?
# What conditions cause a react dot memo child to still re render?
   - React memo only does a shallow prop compare, so new references, object slash arrays slash functions, changing keys or context updates can still trigger re renders.
# In state management terms, what's your rule of thumb for deciding whether a piece of state should live in local component state, React context, or a global store and why?
# What are the main performance pitfalls of using React context for frequently changing state and how do you mitigate them in a large app?
# In your own work, when migrating from context to a global store or vice versa, what concrete signals or pain points trigger that decision?
# How do you structure a global store to avoid everything depends on everything? For example slice boundaries, selectors, normalization, especially as the app grows.
  -Nested providers, re-render churn, and scaling predictability needs are common tipping points.

function curry(fn) {
  const placeholder = curry.placeholder;

  function curried(...args) {
    return function (...nextArgs) {
      let merged = [];
      let nextIndex = 0;

      // fill placeholders
      for (let i = 0; i < args.length; i++) {
        if (args[i] === placeholder && nextIndex < nextArgs.length) {
          merged.push(nextArgs[nextIndex++]);
        } else {
          merged.push(args[i]);
        }
      }

      // append remaining args
      while (nextIndex < nextArgs.length) {
        merged.push(nextArgs[nextIndex++]);
      }

      // count non-placeholders
      const filled = merged.filter(x => x !== placeholder).length;

      if (filled >= fn.length) {
        return fn(...merged.slice(0, fn.length));
      }

      return curried(...merged);
    };
  }

  return curried;
}

curry.placeholder = Symbol("placeholder");