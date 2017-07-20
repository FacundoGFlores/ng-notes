## RXJS Notes

React programming: programming with event streams (sequence of events ocurrying over time). Whenever an event happens with can react to it doing something.

Arrays can be used to simulate events.

**Example 1:** Show events each 400ms.
```
var src = Rx.Observable.interval(400).take(9)
            .map(i => ['event 1', 'event 2', 0, 1, 'event 3'])
src.subscribe(x => console.log(x)) // add event listener to src
```

**Example 2:** Show only integers in the source
```
var src = ['1', 'event 1', '2', '3', 'event 2']
var result = src
                .map(x => parseInt(x))
                .filter(x => !isNaN(x))
//output: [1, 2, 3]
```

**Example 3:** Get the sum of all integers
```
var src = ['1', 'event 1', '2', '3', 'event 2']
var result = src
                .map(x => parseInt(x))
                .filter(x => !isNaN(x))
                .reduce((x, y) => x + y):
//output: 6
```

> Reactive programming allows you to specify the dynamic behaviour of a value completely at the time of creation.




