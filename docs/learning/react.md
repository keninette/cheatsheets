---
tags:
  - react
---

[🏡 Home](../index.md) > [Learning](index.md)

# React

## Side effects
[URL](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25500210#overview)

Everything that is not about rendering UI or reacting to user input:
- Store data
- Send HTTP requests
- Manage timers

### UseEffect hook
[URL](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599212#overview)

```js
useEffect(myFunction, [dependencies]);
```

- `myFunction`: a function that will be executed AFTER every evaluation of the component IF the specified dependencies have changed
  - You can return a function (which is called a Cleanup function), that will be executed BEFORE the function passed as argument runs (except for the very first time);
- `[dependencies]`: array of dependencies. If it's empty, it will only be executed the very first time the component is evaluation.

## Reducers

### UseReducer hook
[URL](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599224#overview)

It's an alternative to `useState` if we need a more powerful state management, for examples if some states
depends on one another.

```js
const [state, dispatchFunction] = useReducer(reducerFunction, initialState, initFunction);
```

- `state`: latest state snapshot
- `dispatchFunction`: a function that will dispatch a new action, that will be consummed by `reductionFunction`
- `reductionFunction`: a function that gets the latest state snapshot & the action dispatched & returns an updated state.
- `initialState`: the initial state
- `initFunction`: an init function for the state in case it's complex

## Context
[URL](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599246#overview)

It is used when you forward state to multiple components, it can also be called store.

### Create your context
```js
const DoctorContext = React.createContext({nb: 11, name: 'Matt Smith'});

export default DoctorContext;
```

### Provide it to your components
In order to tap into a context, you'll need your context to wrap your components. Then it will be available in your components & all their children.

```js
const [bestDoctor, setBestDoctor] = useState();

<React.fragment>
  <DoctorContext.Provider value={bestDoctor}>
    <Header></Header>
    <Body></Body>
    <Footer></Footer>
  </DoctorContext.Provider>
</React.fragment>
```

### Listen to it
#### With the consumer
Wrapping your components with the consumer will allow your components to tap into your context. They should be returned by the function used by your context.

```js
// In parent
return (
  <div>
    <DoctorContext.Consumer>
      {(context) => {
        return (
          <BestDoctorBanner></BestDoctorBanner>
          <AllDoctors></AllDoctors>
        );
      }}
    </DoctorContext.Consumer>
  </div>
);

// In child
return (<div>Best doctor is #{context.nb} {context.name}</div>);
```

#### With a hook
