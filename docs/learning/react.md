---
tags:
  - react
---

[🏡 Home](../index.md) > [Learning](index.md) > ReactJs

# ⚛️ React

## 🍕 React fragments or empty tags

When we render a component, React expects all of our content to be contained in a single element (because behind the scene, when we use JSX, it uses the `createElement` method). As a matter of consequence, we need to add a wrapper, to contain our elements (like a `div` tag for example).

But, most of the time, we don't want to create a wrapper just for this purpose, which is where React fragments come in. `<React.Fragment></>` acts as a wrapper without creating unwanted tags in the DOM. The empty tags notation (`<></>`) is just a shortcut to write these fragments.

### Sources
[A medium article about that](https://medium.com/fasal-engineering/what-are-react-fragments-or-the-react-empty-tags-190253582905)

## 🟠🔵 Portals
Portals are a way to render some children into a different part of the DOM.

They are used to :
- Render to a different part of the DOM.
- Render a modal dialog inside a Modal component that can be displayed on the whole page with an overlay.
- Render React components into non-React server markup
- Render React components in non-React DOM nodes

### Sources
[React.dev](https://react.dev/reference/react-dom/createPortal)


## 🛂 Controlled & uncontrolled components
A controlled component is a component where the value & changes to the value are not handled in the component itself but in its parent component.
The parent component controls the child component.

In a form, a component is controlled when its value & changes are handled by a state, as opposed to the DOM (using `ref`).
An uncontrolled component's value is not controlled by React but relies on the default behaviour of the DOM Element.

### Stateless & Stateful components

AKA:
- Presentational vs stateful components
- Dumb vs smart components

### Sources
[Udemy course part 1](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture25596066#overview)

[Udemy course part 2](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture25598572#overview)

[Medium article about controlled components](https://itnext.io/controlled-vs-uncontrolled-components-in-react-5cd13b2075f9)

## 🤮 Side effects
Everything that is not about rendering UI or reacting to user input:
- Store data
- Send HTTP requests
- Manage timers

### UseEffect hook
```js
useEffect(myFunction, [dependencies]);
```

- `myFunction`: a function that will be executed AFTER every evaluation of the component IF the specified dependencies have changed
  - You can return a function (which is called a Cleanup function), that will be executed BEFORE the function passed as argument runs (except for the very first time);
- `[dependencies]`: array of dependencies. If it's empty, it will only be executed the very first time the component is evaluation.

### Sources
[Udemy course part 1](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25500210#overview)

[Udemy course part 2](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599212#overview)

## 🤏 Reducers

### UseReducer hook

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

### Sources
[Udemy course](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599224#overview)

## 👜 Context

It is used when you forward state to multiple components, it can also be called store.

### Create your context
```js
const DoctorContext = React.createContext({nb: 11, name: 'Matt Smith'});

export default DoctorContext;
```

### Provide it to your components
In order to tap into a context, you'll need your context to wrap your components. Then it will be available in your components & all their children.

```js
const [currentDoctor, setCurrentDoctor] = useState();

<React.fragment>
  <DoctorContext.Provider value={currentDoctor}>
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
return (<div>Current doctor is #{context.nb} {context.name}</div>);
```

#### With a hook

- Remove the consumer & use the `useContext()` hook.

```js
const DoctorContext = React.createContext({nb: 11, name: 'Matt Smith'});

export default DoctorContext;
```

```js
const [currentDoctor, setCurrentDoctor] = useState();
const onRegeneration = () => {
  const doctors = [
    { nb: 9, name: 'Christopher Eccleston'},
    { nb: 10, name: 'David Tennant'},
    { nb: 11, name: 'Matt Smith'},
    { nb: 12, name: 'Peter Capaldi'},
    { nb: 13, name: 'Joddie Whittaker'},
    { nb: 14, name: 'Ncuti Gatwa'},
  ];
  
  const newDoctor = doctors.find((doctor) => doctor.nb === currentDoctor.nb +1);
  
  setCurrentDoctor(newDoctor);
};

<React.fragment>
  <DoctorContext.Provider value={{currentDoctor, onRegeneration}>
    <Header></Header>
    <Body></Body>
    <Footer></Footer>
  </DoctorContext.Provider>
</React.fragment>
```

```js
// In parent
return (
  <div>
    {(context) => {
      return (
        <BestDoctorBanner></BestDoctorBanner>
        <AllDoctors></AllDoctors>
      );
    }}
  </div>
);

// In child
import React, { useContext } from 'react';
import DoctorContext from '../../store/doctor-context';

const context = useContext(DoctorContext);

return (<div>
  Current doctor is #{context.nb} {context.name}
  <Button onClick={context.onRegeneration()}>I don't want to go</Button>
</div>);
```

### Sources
[Udemy course](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599246#overview)

## 🐕 Fetch data

In order to fetch data, we can either use `fetch()`, which is provided by any browser, or use the `axios` bundle.



### Sources
[Udemy course part 1](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25599796#overview)

## 📚️ Sources
- [Udemy react course](https://circle.udemy.com/course/react-the-complete-guide-incl-redux/learn/lecture/25595340#overview)
