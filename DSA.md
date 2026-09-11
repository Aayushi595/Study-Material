Sliding window protocol : 
- process a series of data elements. Input is list, array, 
- 


# Fuction Curry with Placeholder - Algorith.
curriedJoin(1, _, _)(_, 3)(2) => 1_2_3
curriedJoin(1, _)(_, _, 3)(2) => Final merged array - [1, 2, _,3] - According to BFE , this will return a function and not string and will not fail the tc.

1. Iterate on old args. i = 0 and new args j = 0

2. if oldarg[i] != placeholder 
{
   i++;                         //skip 
}else {
    ordArg[i++] = newArg[j++]     //replace and continue
}
append remaining arg from newArgs.

function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length && args.slice(0, func.length).every(x => x!== "-")) {
      return func(...args);
    } 
      
      return function(...nextArgs) {
        let searchindex = 0;
        let j = 0;
        let filled = false;
        while (j < nextArgs.length){
  for(i = searchindex; i < args.length ; i++){
    if (args[i] == "-"){
       args[i] = nextArgs[j++]
       searchindex = i + 1
       filled = true;
       break;
    }
    if (!filled) break; // ← inside for, breaks for-loop not while-loop
  }
  // while loops again forever if no placeholder found
}
        let remainingArgs = nextArgs.slice(j, nextArgs.length )
        console.log('remaining', remainingArgs)
                console.log(...args, ...remainingArgs, "final Array")

        return curried(...args, ...remainingArgs);
      }
    
  };

}

const  join = (a, b, c) => {
    console.log(`${a}_${b}_${c}`)
   return `${a}_${b}_${c}`
   
}
const curriedJoin = curry(join)
curriedJoin(1, "-")("-",3)