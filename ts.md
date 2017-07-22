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

#### Types definitions

```
type HasName = {firstName: string, lastName: string};

let person: HasName = {
  firstName: 'John',
  lastName: 'Doe'
}
```

When you omit in `person` some of the properties declared in `HasName` type a compilation error will occur, so if you want to allow the `person` object to not declare some property you can use `?` in the type property declaration.

```
type HasName = {firstName?: string, lastName: string};
```

> This feature allows you autocompletion.

Now, you can compose types:

```
type HasAddress = {
  street: string;


type Person = {
  name: HasName,
  address?: HasAddress
}

let person: Person = {
  name: {
    lastName: 'Doe'
  }
}
```

#### Union and Insersection

```
type Person = (HasName & HasAddress) | null;
```

> Important: The type `undefined` you really probably never want to use in your program directly, you probably want to define things like the `Person` type which combines either primitives types or app defined types such as `HasAddress` or `HasName` and `null`, so this explicitly excludes the type `undefined`, because `null` is not `undefined`.

#### Generics

A tool for creating reusable components, that is, being able to create a component that can work over a variety of types rather than a single one. This allows users to consume these components and use their own types.

Non-Generic
```
let users: Users[] = [{}, {}, {}]
```

Generic
```
let users: Array<Users> = [{}, {}, {}]
```

So for example you may have:

```
function identity(arg: any): any {
  return arg;
}
```

Using `any` is certainly generic in the sense it accepts any and all types for the type of `arg`, we actually are losing the information about what that type was when the function returns. If we passed in a number, the only information we have is that any type could be returned.

Instead, we need a way of capturing the type of the argument in such a way that we can alaso use it to denote what is being returned. So, we use a *type variable*, a special kind of variable that works on types rather than values.

```
function identity <T>(arg: T): T {
  return arg;
}
```

This allows us to traffic that type information in one side of the function and out the other.

We say that this version of the `identity` function is generic, as it works over a range of types. Unlike using `any`, it's also just as precise (ie, it doesn't lose any information) as the first `identity` function that used numbers for the argument and return type.

Now we can call the functionin one of two ways.

1. To pass all of the arguments, including the type argument, to the function:

```
let output = identity<string>("hello world")
```

We explicitly set `T` to be `string` as one of the arguments to the function call, denoted using `<>` around the arguments rather than `()`.

2. We use *type argument interface*, that is, we want the compiler to set the value of `T` for us automatically based on the type of the argument we pass in:

```
let output = identity("hello world)
```

#### Iterators

Iterator itself is not a TypeScript or ES6 feature, Iterator is a Behavioral Design Pattern common for Object oriented programming languages. It is, generally, an object which implements the following interface:

```
interface Iterator<T> {
    next(value?: any): IteratorResult<T>;
    return?(value?: any): IteratorResult<T>;
    throw?(e?: any): IteratorResult<T>;
}
```

This interface allows to retrieve a value from some collection or sequence which belongs to the object.
The `IteratorResult` is simply a `value` + `done` pair:

```
interface IteratorResult<T> {
    done: boolean;
    value: T;
}
```

#### Generators

They can be used to create lazy iterators e.g. the following function returns an infinite list of integers on demand:

```
function* infiniteSequence() {
    var i = 0;
    while(true) {
        yield i++;
    }
}

var iterator = infiniteSequence();
while (true) {
    console.log(iterator.next()); // { value: xxxx, done: false } forever and ever
}
```

if the iterator does end, you get the result of `{done: true}`:

```
function* idMaker(){
  let index = 0;
  while(index < 3)
    yield index++;
}

let gen = idMaker();

console.log(gen.next()); // { value: 0, done: false }
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { done: true }
```



