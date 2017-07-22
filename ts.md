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

> IMPORTANT: it doesn't work as we expect with objects. Only the object reference is immutable.

```
const obj = {
  flag: true
}
obj.flag = false //allowed
obj = null // not allowed because the reference is const
```

#### Object Destructuring

When you want to build some data, you probably do something like:
```
function buildUserInfo(user) {
  return user.userName + ' ' + user.email + ' ' + user.profileLink;
}

const userData = {
  userName: 'JohnDoe',
  email: 'john.doe@corporation.com',
  profileLink: 'http://corporation.com/profiles/1'
};

const userInfo = buildUserInfo(userData);
console.log(userInfo);
```

But it would better to use string literals.

```
function buildUserInfo(user) {
  const userName = user.Username,
        email = user.email,
        profileLink = user.profileLink;
  return `${userName} ${email} ${profileLink}`
}
```

But with destructuring provides a more elegant solution

```
function buildUserInfo({userName, email, profileLink}) {
  return `${userName} ${email} ${profileLink}`
}
```

note that destructuring only supplies the properties of the object that we need.


