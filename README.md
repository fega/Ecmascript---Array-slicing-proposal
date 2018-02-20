# Ecmascript---Array-slicing-proposal
A proposal to a better array Slicing inspired in Python notation
## Problem
## Solution
## Applications
#### Intuitive array slicing
```
const arr=[0,1,2,3,4,5];

const slice=arr[1:4]; // [1,2,3,4];
```

#### Array Reversing
```
const arr=[0,1,2,3,4,5];

const slice=arr[1:4:-1]; // [4,3,2,1];
```

#### variable swaping

```
let arr=[0,1,2,3,4,5];

arr=arr[1:2]=arr[1:2:-1] // arr= [0,2,1,3,4,5]
```

#### Nested Arrays

```
const mat=[[1,2,3,4,5],
           [5,6,7,8,9]
           [0,1,2,3,4]];

const mat[0:1][2:3]=// [[3,4]
                    //  [7,8]]
```
#### Spread Operator?
