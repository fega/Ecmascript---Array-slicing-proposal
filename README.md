# Ecmascript-Array-slicing-proposal
A proposal to a better array Slicing inspired in Python notation
## Problem
JS need a better way to transversing arraylike structures
## Solution
Inspired (but not limited) in the python notation, add an `slicing notation`
in the following form
```
arr[start:end:step]
```
Where 
 - *start* is the start index of the slice, default is Zero
 - *end* is the end index of the slice (and at difference of python it includes that index), default is arr.length
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
