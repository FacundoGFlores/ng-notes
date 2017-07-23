# NG-NOTES

### Lifecycle Hooks

#### OnInit()

1. To perform complex initializations shortly after construction
2. To set up the component after Angular sets the input properties (constructor)

What not to put in the constructor
```
new keyword
static method calls
initialize methods / blocks
control flow
fetch data
```

Why not to put those stuff
```
The violates the Single Responsability Principle
Testing directly is difficult (too much load such as: files, apis, database)
```

#### OnDestroy()

1. Perform cleanup logic
2. Free resources that won't be garbage collected automatically. 
3. Unsubscribe from Observables and DOM events.
4. Stop interval timers.

Sample: 
```
ngOnInit() {
  this.uploadFileService.percentUpload$.subscribe({
    next: (v) => this.percentComplete = v
  });
}

ngOnDestroy() {
  this.uploadFileService.percentUpload$.unsubscribe();
}
```

#### OnChanges()

Angular calls it whenever it detects changes to **input** properties of the component. The `ngOnChanges()` method takes and object that maps each changed property name to a `SimpleChange` object holding the current and previous property values.

Sample:

```
ngOnChanges(changes: SimpleChanges) {
  for (let propName in changes) {
    let chng = changes[propName];
    let cur  = JSON.stringify(chng.currentValue);
    let prev = JSON.stringify(chng.previousValue);
    this.changeLog.push(`${propName}: currentValue = ${cur}, previousValue = ${prev}`);
  }
}
```

> IMPORTANT: Angular only calls the hook when the value of the input property changes. It means when you pass and object `person` to the component and the last one changes the person's `name` property, angular won't report it as a change.

#### DoCheck()

Used to act upon changes that Angular doesn't catch on its own. We can say that it exceds the `OnChanges` hook.

> You should not use both `DoCheck` and `OnChanges` to respond to changes on the same input.

Sample:
```
ngDoCheck() {

  if (this.hero.name !== this.oldHeroName) {
    this.changeDetected = true;
    this.changeLog.push(`DoCheck: Hero name changed to "${this.hero.name}" from "${this.oldHeroName}"`);
    this.oldHeroName = this.hero.name;
  }
  //...
}
```