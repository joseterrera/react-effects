### UseEffect
- React comes with a built in hook for “side effects” (Fetching data, starting a timer, and manually changing the DOM are all side effects)
- Each render has its own effects
- Sometimes these effects require clean-up (clearing a timeout, closing a connection)
- useEffect will run after the first render
- useEffect will run after all rerenders by default
- useEffect accepts a callback function as its first argument
- useEffect returns undefined or a function
- If you return a function, the function will be run before the component unmounts or before the effect runs again

### useEffect arguments
#### 2nd argument to useEffect
- You can tell React to skip applying an effect if certain values haven’t changed between re-renders.
- useEffect accepts an array as its second argument where you place these values (also called dependencies)
- What you pass to the array can help avoid performance issues (we’ll talk about this more later)
- If you want to run an effect and clean it up only once (on mount and unmount), you can pass an empty array ([]) as a second argument.

- This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run.


### Fetching Data on Initial Render
#### A Typical Use Case for useEffect
- It’s very common that when a component renders, you’ll want to fetch some data from an external data source or API
- We want to do this after the component first renders so that we can show the user something (e.g. a loading screen) while we fetch that data
- To fetch correctly, we’ll run an effect that only happens once and when the API call is finished, we’ll set our state and render the component again
- Some important notes here:

  - useEffect cannot be an async function, we must define an async function inside and invoke it
  - make sure that we change state after getting back a response

Cleaning up an Effect

```js
useEffect(() => {
  // runs on the first render and all times after
  // because we didn't pass in an array as a 2nd arg!
  console.log('Effect ran!');

  // if we return a function
  // it will run when the component unmounts
  // or before the effect runs again
  return () => console.log('in the cleanup phase!');
})
```

### useRef

useRef is another built-in hook in React.
It returns a mutable object with a key of current, whose value is equal to the initial value passed into the hook.
The object persists across renders (so it’s like a local variable, but independent of state).
Mutating the object does not trigger a re-render.
Common Applications of useRef
Accessing an underlying DOM element
Setting up / clearing timers
Accessing the DOM
Sometimes, you need to access an HTMLElement to make use of a Web API or to integrate some other JavaScript library.

This is a great time to use a ref!

Accessing the DOM Example
.playbackRate can only be changed if you have access to the underlying HTML element.
A ref can get us access to the DOM element!
To assign a ref to a DOM element, use the ref attribute on the desired element.
Timers
Another great time to use a ref is with timers like setInterval.

setInterval returns a timer ID, which we need to stop the setInterval from running.

We can store that ID in a ref, and then stop the timer when the component is removed from the DOM.

Timer Example
Antipattern for useRef
Since refs can expose DOM elements for us, it can be tempting to use them to access the DOM and make changes (toggle classes, set text, etc).

This is not how refs should be used. React should control the state of the DOM!

From the docs:

Your first inclination may be to use refs to “make things happen” in your app. If this is the case, take a moment and think more critically about where state should be owned in the component hierarchy.


