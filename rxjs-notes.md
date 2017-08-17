## RXJS Notes

React programming: programming with event streams (sequence of events ocurrying over time). Whenever an event happens with can react to it doing something.

Some features for Observables

1. Observables is lazily evaluated that can synchronously or asynchronously return zero to (potentially) infinite values from the time it's invoked onwards.
2. Observables are like functions with zero arguments, but generalize those to allow multiple values.
3. Subscribing to an Observable is analogous to calling a function

```javascript
// function approach
function foo() {
  console.log('Hello World!');
  return 150;
}
var x = foo.call();
console.log(x);

// observable approach
var foo = Rx.Observable.create(function (observer) {
  console.log('Hello World!');
  observer.next(150);
});
foo.subscribe(function (x) {
  console.log(x);
});

// response
// "Hello World!"
// "150"
// "Hello World!"
// "150"
```

### Working with Operators

**Example 0:** Generate random infinite random numbers
```javascript
const observable = new Rx.Observable(observer => {
  (function generateRandomNumber() {
    setTimeout(
      () => {
        observer.next(Math.random());
        generateRandomNumber();
      },
      1000
    );
  })();
});
```

Or maybe you just want to use `rxjs`
```javascript
Rx.Observable.interval(1000).take(Infinity).map(() => Math.random())
```

If you were comfortable with promises

```javascript
promise.then(
  successFn,
  errorFn
)

observable.subscribe(
  nextFn,
  errorFn,
  completeFn
)
```

> without operators, observables are no scarier than promises

So, what operator do I use?

1. Use operator guide at: http://reactivex.io/rxjs
2. Remember you don't have to Rx everything
3. Take the operators you know to whatever point you can and do the rest imperatively.

Start with: `map, filter, scan, mergeMap, switchMap, combineLatest, cocant, do`

**Example 1:** Show events each 400ms.
```javascript
var src = Rx.Observable.interval(400).take(9)
            .map(i => ['event 1', 'event 2', 0, 1, 'event 3'])
src.subscribe(x => console.log(x)) // add event listener to src
```

**Example 2:** Show only integers in the source
```javascript
var src = ['1', 'event 1', '2', '3', 'event 2']
var result = src
                .map(x => parseInt(x))
                .filter(x => !isNaN(x))
//output: [1, 2, 3]
```

**Example 3:** Get the sum of all integers
```javascript
var src = ['1', 'event 1', '2', '3', 'event 2']
var result = src
                .map(x => parseInt(x))
                .filter(x => !isNaN(x))
                .reduce((x, y) => x + y):
//output: 6
```

> Reactive programming allows you to specify the dynamic behaviour of a value completely at the time of creation.

**Example 4:** Using promise

```javascript
const usersPromise = fetch('/users').then(res => res.json())
const users$ = Observable.fromPromise(usersPromise)
```

We can pass arguments to the `subscribe` function:

```javascript
  users$.subscribe(
    users => ProcessNextValue(users),
    error => HandleError(error),
    () => HandleCompletion(),
  )
```

**Example 5:** Creating observable from another observable
```javascript
const firstUser$ = users$.map(users => users[0])
```
If we subscribe to the last observable the next value will be just one user.

**Example 6:** Using Angular HTTP
```javascript
const request$ = http.get('/users')
```
Here `request$` is an Observable so we can subscribe to it
```javascript
request$.subscribe(
  response => console.log(response)
)
```

But most of the times we really want the data, for that we use the `map` operator
```javascript
const users$ = http.get('/users').map(response => response.json())
users$.subscribe(
  users => console.log(users)
  () => {},
  () => console.log("completed!")
)
```

**Example 7:** Using Observables with Angular Services

Component
```javascript
users: Users[];
constructor(usersService: UsersService){}
ngOnInit(){
  users$ = usersService.getUsers();
  users$.subscribe(users => this.users = users);
}
```

UsersService
```javascript
getUsers(): Observable<User[]> {
  return this.http.get('/users').map(response => response.json())
}
```

**Example 8:** Multiple requests I

Suppose we want to send a new user to the server, we will probably want to refresh the list of users.
```javascript
const addUser$ = this.usersService.addUser(user);
const reloadUsers$ = this.usersService.getUsers();
const updateUsers$ = Observable.concat(
  addUser$,
  reloadUsers$
);
updateUsers$.subscribe(
  () => {},
  () => {},
  () => {
    this.users$ = reloadUsers$;
  },
)
```
NOTE: `async` pipe will automatically subscribe to the `users$` observable

```javascript
<users-list [users]="users$ | async"><users-list>
```

>IMPORTANT: if you are concatenating more than one Observables and you are using the `async` pipe, then you should probably need to add a `cache()` operator to avoid doing multiple subscriptions.

**Example 9:** Multiple requests II

```javascript
this.users$ = this.usersService.addUser(user1)
  .switchMap(result => this.usersService.addUser(user2))
  .switchMap(() => this.usersService.getUsers())
  .cache(); // avoid multiple GET
```

**Example 10:** Merge vs Concat

Merge
```javascript
squares$ = Rx.Observable.interval(1000).take(5).map(i => {return "square:" + i * i})
cubes$ = Rx.Observable.interval(500).take(5).map(i => {return "cube:" + i * i * i})
Rx.Observable.merge(squares$, cubes$).subscribe(console.log)
// output
// cube:0
// square:0
// cube:1
// cube:8
// square:1
// cube:27
// cube:64
// square:4
// square:9
// square:16
```

Concat
```javascript
Rx.Observable.concat(squares$, cubes$).subscribe(console.log)
// output
// square:0
// square:1
// square:4
// square:9
// square:16
// cube:0
// cube:1
// cube:8
// cube:27
// cube:64
```
