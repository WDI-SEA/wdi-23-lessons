# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) React Component Lifecycle


### Learning Objectives
*After this lesson, you will be able to:*
- List the three categories of the React component lifecycle
- Define the main methods in each lifecycle category


## React Component Lifecycle

Okay - you've aced it so far. Let's get technical!

React class components provide several lifecycle methods that you can use to control your application based on the state of the UI.

These methods happen automatically - but you can call them to modify them.

The lifecycle falls into three main pieces: When the component is being created (which is called **mounting**), when the component is being **updated**, and when the component is being removed from the DOM (which is called **unmounting**).

These methods are called at specific points in the rendering process. You can use these methods to perform actions based on what's happening on the DOM.

* `componentDidMount`, for example, is called immediately *after* a component is rendered to the DOM.
* `componentWillUnmount` is called immediately *before* a component is removed from the DOM.

Some common uses of lifecycle methods are making asynchronous requests, binding event listeners, and optimizing for performance.

## At a very high level

React components' lifecycle events fall into three broad categories:

* **Initializing / Mounting** e.g. What happens when the component is created and inserted into the DOM? Was an initial state set? Methods:
  - `constructor()`
    - This is sometimes referred to as a combination of `getInitialState()` and `getDefaultProps()`
  - `componentWillMount()`
  - `componentDidMount()`
  - `render()`


* **Updating** e.g. Did an event happen that changed the state? What happens when a component is being re-rendered? Methods:
  - `componentWillReceiveProps()`
  - `shouldComponentUpdate()`
  - `componentWillUpdate()`
  - `componentDidUpdate()`
  - `render()`


* **Destruction / Unmounting** e.g. What needs to happen when we're done with the component? Method:
  - `componentWillUnmount()`


## Let's examine!
Let's go through them. Again, you don't need to write these methods - they happen automatically, just like constructors did before we explicitly wrote one. You only have to worry about these if you want to change something in them - but if you do, it's important to understand them!

Here they are in a diagram - ![Lifecycles](./assets/React_Component_Lifecycle.png)

### The `constructor()` Method

This is something we've already seen when we were learning `state`. This is in the first part of the component lifecycle- **initializing/mounting**. Like any JavaScript class, the `constructor` method is called when a component is instantiated (when it's first rendered).
In a class constructor, you must call `super` before you do anything else. So a React component `constructor` in its most basic form looks like this:

```javascript
constructor(props) {
  super(props);
}
```

You don't need to define a constructor if that's all it does, though. This happens automatically when your component is invoked. A common use of the constructor is to initialize `state` using the `props` passed to the component - as we have been doing!

```javascript
constructor(props) {
  super(props)
  this.state = {
    fruits: props.fruits
  }
}
```

> This constructor sets the initial `fruits` `state` of the component to the `fruits` `prop` passed to the component. Then, using `setState`, your user can add fruits, delete them, or whatever else your component allows.

Another common use of the `constructor` method is to bind class methods.

When a method is "bound" to a component, it means that when the method is invoked, `this` will refer to the component, and not to the context that invoked the method. So, if you pass a component's method to a child component from a parent component, you can be sure that `this.setState` will set the `state` of the parent component if the method is bound to it.

For example, we could set
```js
this.myFunctionName = this.myFunctionName.bind(this)
```

In JavaScript classes, methods aren't bound by default. So, if you pass a component's method to a child component _without_ binding it, it can lose its context, and not behave the way you intended. Here's an example:

```javascript
class FruitTable extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      fruits: props.fruits
    }
    this.addFruit = this.addFruit.bind(this)
  }

  addFruit(fruit) {
    this.setState(prevState => ({
      fruits: prevState.fruits.concat(fruit)
    }))
  }
}
```

Notice that in the constructor, `this.addFruit` is bound to the class:
`this.addFruit = this.addFruit.bind(this);`

Now, if we pass `this.addFruit` to a child component as an `onChange` callback, it will be bound to `FruitTable` and will update its state when it's invoked.

Again, you _don't_ need a constructor in every React component, but if you need to initialize `state` by `props` or `bind` methods, the constructor is where you do it.

### The `componentWillMount()` Method

This method is called immediately before a component is rendered to the DOM. You generally won't need to use this method. Advanced use-cases like server-rendering are usually the only ones in which `componentWillMount` is needed.

> **Note:** `componentWillMount` shouldn't be used for server requests because it may be invoked multiple times before the render in future versions of React. [Side effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science) should be avoided in the `constructor`, and so server requests shouldn't be made there. The accepted answer on [this Stack Overflow](http://stackoverflow.com/questions/41612200/in-react-js-should-i-make-my-initial-network-request-in-componentwillmount-or-co) from a member of the React team at Facebook gives more detail.


### The `componentDidMount()` and `componentWillUnmount()` Methods

The `componentDidMount` method is called once, immediately after your component is rendered to the DOM. If you want to make an AJAX request when your component first renders, this is where to do it (_not_ in the constructor, or in `componentWillMount`).

In the following example, we fetch data from the server, then set the state of the component using the response.

```javascript
componentDidMount() {
  fetch(`http://api.com/${this.props.id}`)
    .then(response => response.json())
    .then(data => this.setState(prevState => ({ data })))
}
```

Another common use for `componentDidMount` is to bind event listeners to your component. You can then remove the event listeners in `componentWillUnmount`, which is the only method in the last part of the component lifecycle- when the component is being removed from the DOM.
In this example, we bind and unbind an event listener for a drag-drop component.

```javascript
class FruitTable extends React.component {

  componentDidMount() {
    document.addEventListener('dragover', this.handleDragStart)
  }

  componentWillUnmount() {
    document.removeEventListener('dragover', this.handleDragStart)
  }

  handleDragStart(e) {
    e.preventDefault()
    this.setState(prevState => ({
      dragging: true
    }))
  }
}
```

### The `render()` Method

This is the one method that every React class component **must** have. It is called in two parts of the component lifecycle - at the beginning, when the component is being initiated/mounted, and when the component is being updated.
- In `render`, you return JSX using the compoment's `props` and `state`.
- You should never set `state` in `render` - `render` should only react to changes in `state` or `props`, not create those changes.

The following component renders a single `prop` and a single `state` key - a car model and a speed. Once this component is mounted, its `speed` state will increase by 1 every second. You can [try it out yourself on CodePen](https://codepen.io/SuperTernary/pen/zzMPGp).

```javascript
class Car extends React.Component {

  constructor(props) {
    super(props)

    // sets initial state to either the `speed` prop, or 0 if the `speed` prop
    // is not passed to the component
    this.state = {
      speed: props.speed || 0
    }

    // binds this.incrementSpeed to class
    // this way when it's used as a callback for `setTimeout`
    // `this` refers to the `Car` class
    this.incrementSpeed = this.incrementSpeed.bind(this)
  }

  componentDidMount() {
    // calls this.incrementSpeed after one second
    window.setTimeout(this.incrementSpeed, 1000)
  }

  incrementSpeed() {
    // sets the `speed` state to one unit higher than it was previously
    this.setState(prevState => ({ speed: prevState.speed + 1 }))

    // recursive method
    // increases speed by 1 again after one second
    window.setTimeout(this.incrementSpeed, 1000)
  }

  render() {
    return (
      <div>
        This {this.props.model} is going {this.state.speed} miles per hour!
      </div>
    )
  }

}
```

To break down what's happening in this component:

- `constructor`

The initial state is set to either the `speed` prop, or 0 if the `speed` prop is not passed to the component.

We bind `this.incrementSpeed` to the class, so that when it's used as a callback for `setTimeout`, `this` refers to the `Car` class.

- `componentDidMount`

Use [`window.setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) to call `this.incrementSpeed` after one second (1000 ms).

- `incrementSpeed`

`this.setState(prevState => ({ speed: prevState.speed + 1 }))`: The `speed` state is set to one higher than it was previously - we add one.

`window.setTimeout(this.incrementSpeed, 1000)`: The `incrementSpeed` method is [recursive](https://en.wikipedia.org/wiki/Recursion_(computer_science) - it invokes itself as the timeout callback. After one second, `window.setTimeout` will call `this.incrementSpeed` again - the `speed` will go up by one, and a new timer will be set to do it again.


### The `componentWillReceiveProps(newProps)` Method

This method is called any time your component receives new `props`. It is _not_ called with the initial `props` when your component initially mounts. If you need to change the state of your component based on changes in the `props`, this is where you do it.

In a simple app, you generally won't need `componentWillReceiveProps`.

### The `shouldComponentUpdate`, `componentWillUpdate`, and `componentDidUpdate` Methods

These methods are called when a component's `props` or `state` change, and are generally used for performance optimizations. React is quite fast by itself, and you usually don't need to concern yourself with these methods outside of a large app with many dynamic components.

## Summary

React class components have lifecycle methods that are invoked at certain stages of a component's "life" on the DOM. Some of the lifecycle methods you'll use frequently include:

    - `constructor()`: Initializes state, binds methods.
    - `componentDidMount()`: Makes AJAX requests, gets DOM refs, binds event listeners, sets `state` if necessary.
    - `componentWillUnmount()`: Unbinds event listeners, performs other clean up.
    - `componentWillReceiveProps()`: Updates `state` based on changes in components.
    - `render()`: Returns markup/UI.