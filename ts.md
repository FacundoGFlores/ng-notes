## Typescript Notes

#### Let vs Const

The problem of `var`
```
var flag = true
if(flag) {
  var i = 0
  i++
  i++
  console.log("Inside if ", i)
}

console.log("Outside if" , i) //can access to "local" i
```
One way to solve this problem is using `let`
```
let flag = true
if(flag) {
  let i = 0
  i++
  i++
  console.log("Inside if ", i)
}

console.log("Outside if" , i) // error: i is not defined
```

One of the best practices to let programs easy to read and mantain is to avoid mutable states.

```
const flag = true
flag = false // error flag was previously declared
```

> IMPORTANT: it doesn't work as we expect with objects.

