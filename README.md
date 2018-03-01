# Ecmascript Array slicing proposal
This proposal introduces a new way for transversing arraylike structures
## Problem and solution
In the current ES specification the different ways to get subsets of arrays are verbose especially if your desire is to get non-sequential subsets of them ie:
```
const arr=[0,1,2,3,4,5];

// get a secuential slice
arr.slice(1,4); // [1,2,3,4];

// but what if you want a non-sequential slice?
// taken from https://codereview.stackexchange.com/questions/57268/implementing-python-like-slice-in-javascript

function slice(collection, start, end, step) {
    var length = collection.length,
        isString = typeof collection == "string", // IE<9 have issues with accessing strings by indicies ("str"[0] === undefined)
        result = [];
    if (isString) {
        collection = collection.split("");
    }
    if (start == null) {
        start = 0;
    } else if (start < 0) {
        start = length + start;
    }
    if (end == null || end > length) {
        end = length;
    } else if (end < 0) {
        end = length + end;
    }
    if (step == null) {
        step = 1;
    } else if (step === 0) {
        throw "Slice step cannot be zero";
    }
    if (step > 0) {
        for (; start < end; start += step) {
            result.push(collection[start]);
        }
    } else {
        for (end -= 1; start <= end; end += step) {
            result.push(collection[end]);
        }
    }
    // Return a string for input strings otherwise an array
    return isString ? result.join("") : result;
}
slice(arr,1,6,2) // [1,3,5]
```
Inspired (but not limited) in the python notation, this proposal aims to address this in a mover convenient way adding a `slicing notation`, in the following form
```
arr[start:end:step]
```
**Where**
 - *start* is the start index of the slice, default is Zero
 - *end* is the end index of the slice (and different of python it includes that index), default is arr.length
 - *step* is a parameter to "jump" indexes, by default is 1, and throw an error if is 0

All of three parameters are optional and the second `:` is only needed if a step is provided:

```
arr[:] valid
arr[::] valid?
arr[undefined:undefined:undefined] valid
```

## Applications

### Intuitive array 
The most understandable use case
```
const arr=[0,1,2,3,4,5];

arr[1:4]; // [1,2,3,4];
```

### Get first part of Array
Allowing only the use of some of the parameters
```
const arr=[0,1,2,3,4,5];

arr[:1] // [0,1]
```
### Get last part of Array
```
const arr=[0,1,2,3,4,5];

arr[4:] // [4,5]
```

### Get even indexes
```
const arr=[0,1,2,3,4,5];

arr[2::2] // [2,4]
```

### Get odd indexes
```
const arr=[0,1,2,3,4,5];

arr[1::2] // [1,3,5]
```

### Array Reversing
```
const arr=[0,1,2,3,4,5];

arr[1:4:-1]; // [4,3,2,1];
arr[::-1] // [5,4,3,2,1,0]
```

### variable swaping

```
let arr=[0,1,2,3,4,5];

arr[1:2]=arr[1:2:-1] // arr= [0,2,1,3,4,5]
```

### Nested Arrays
This brigs a powerful way to transversing any Matrix of data

```
const mat=[[1,2,3,4,5],
           [5,6,7,8,9]
           [0,1,2,3,4]];

const mat[0:1][2:3]=// [[3,4]
                    //  [7,8]]
```

```
const mat=[[0,1,0,1,0],
           [0,0,0,0,0]
           [0,1,0,1,0]];

const mat[1::2][0::2]=// [[1,1]
                    //    [1,1]]
```
## Other ideas:

### Spread Operator?
for me this is ugly... and unconvenient
```
const arr=[0,1,2,3,4,5];
           ^ ^ ^
           | | step
           | end
           start
arr[...arr] // []
```
### Mathematical notation
```
const arr=[0,1,2,3,4,5];

arr[0:5) // [0,1,2,3,4]
```
### Allow function in third "parameter"
The function will be evaluated every time that a value is returned, and the resulting number will be the next "step"
```
const arr=[0,1,2,3,4,5];
arr [::(value,index,arr,start,end)=>{return something}]
```
**Problem:** could lead in an infinite bucle

### Allow array array in third parameter
with a serie of steps, if the step is negative, be back from the las returned item, when array in parameter ends, restart secuence

```
const arr=[0,1,2,3,4,5,6,7];
arr[::[1,2]] // [0,1,3,4,6]
```
**Problem:** could lead in an infinite bucle

```
const arr=[0,1,2,3,4,5];
arr[::[1,-1]] // ?
```
**Solution:** sum of array should not be Zero

```
const arr=[0,1,2,3,4,5];
arr[::[1,-1]] // throws Error
```
## Dark hole of crazyness
### Allow Arrays in first "parameter"
Hahaha, this is so much for me

```
const arr=[0,1,2,3,4,5];
arr[arr:] //   [[0,1,2,3,4,5],
          //    [1,2,3,4,5],
          //    [2,3,4,5],
          //    [3,4,5],
          //    [4,5],
          //    [5],
          //    ]
```
### Pass [::] as a parameter


```
const arr=[0,1,2,3,4,5];
function getSlice(a,slice){
  return a[slice]
}
getSlice(arr,[0:1]) // [0,1]
getSlice(arr,[0:5)) // [0,1,2,3,4]
```
### a map "parameter"
```
const arr=[0,1,2,3,4,5];
arr[:::(val,index,arr,start,stop,step)=>{return 1}] //[1,1,1,1,1,1]
```
