# Functional Programming Notes

#### Compositions over inheritance

Inheritance: is when you design your *types* after what they *are*.

Composition: is when you design your *types* after what they *can do*.

Gorilla banana's problem: You request a banana but instead you get a gorilla holding a banana.

Intheritance: Encourages you to predict the future.

In most scenarios you probably created things like:

```
Person
  .eat()
  .breath()
  .talk()
  .walk()
  
  Student
    .learn()

  Professor
    .teach()
```

And then, your client cames to you to request a new feature: "a robot professor".
The first thing you think is to create:

```
Robot
  .loadBattery()
  .loadDataFromInternet()
```

And maybe you thought in "inheritance", *a robot IS a professor* (note how we are using the definition of inheritance). *Here comes the... problem*: the thing is a robot should not breath or eat. Composition to the rescue!

```
robotProfessor = professor + robot
```

You may code something like:

```
const professor = (state) => ({
  teach: () =>  console.log(`I'm teaching ${state.subject}`)
})
const robot = (state) => ({
  loadBattery: () => {
    state.battery++;
    console.log(`I'm loading batteries! ${state.battery}%`);
  }
})

const robotProfessor = (subject) => {
  let state = {
    subject,
    battery: 0
  }
  return Object.assign(
    {},
    professor(state),
    robot(state),
  )
}

const historyRobot = robotProfessor('history');
historyRobot.teach(); // I'm teaching history
historyRobot.loadBattery(); // I'm loading batteries! 1%
historyRobot.loadBattery(); // I'm loading batteries! 2%
```

