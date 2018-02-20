# Ecmascript-Array-slicing-proposal
A proposal to a better array Slicing inspired in Python notation
## Problem
JS need a better way to transversing arraylike structures
## Solution
Inspired (but not limited) in python notation, add an `slicing notation`
## Applications

### Get first part of Array
### Get last part of Array
### Get even indexes
### Get odd indexes

### Intuitive array slicing
```
const arr=[0,1,2,3,4,5];

const slice=arr[1:4]; // [1,2,3,4];
```

### Array Reversing
```
const arr=[0,1,2,3,4,5];

const slice=arr[1:4:-1]; // [4,3,2,1];
```

### variable swaping

```
let arr=[0,1,2,3,4,5];

arr[1:2]=arr[1:2:-1] // arr= [0,2,1,3,4,5]
```

### Nested Arrays

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
           | | stop
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
### Allow two parameters in first "parameter"
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
