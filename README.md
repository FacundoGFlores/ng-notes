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
