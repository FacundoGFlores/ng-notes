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

### Smart Components vs Presentation Components

The main difference between those it the form they receive data from Services. Smart Components use constructor injection to lookup the entire service from the injector, while Presentation Components take the data from `@Inject` defined on their component class. Presentation components use inputs for passing data in, and these smart containers use the constructor for dependency injection to pass the services in.

### Directives

There are three types of directives in Angular:

1. Attribute: Change the apprearance or behavior of an element.
2. Structural: Change the DOM adding and removing DOM elements.
3. Components: Directives that have a template.

#### Structural Directives

They are prefixed with an asterisk when being used which signals to Angular that the structure of the DOM elements within the directive may change depending on certain conditions. 

> Why remove rather than hide? The difference between hiding and removing doesn't matter for displaying some fixed content. It does matter when the host element is attached to a resource intensive component. Such a component's behavior continues even when hidden. The component stays attached to its DOM element. It keeps listening to events. Angular keeps checking for changes that could affect data bindings. Whatever the component was doing, it keeps doing. Although invisible, the component—and all of its descendant components—tie up resources. The performance and memory burden can be substantial, responsiveness can degrade, and the user sees nothing. On the positive side, showing the element again is quick. The component's previous state is preserved and ready to display. The component doesn't re-initialize—an operation that could be expensive. So hiding and showing is sometimes the right thing to do.

#### Attribute Directives

They are surrounded by brackets which signals to Angular that the appearance or behavior of the DOM elements within the directive may change depending on certain conditions.