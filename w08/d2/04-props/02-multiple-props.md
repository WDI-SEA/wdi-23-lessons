# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Multiple Props


### Learning Objectives
*After this lesson, you will be able to:*
- Pass multiple individual props to a component
- Pass multiple props as an object to a component

## What about... multiple props?

Of course, we often want components to display more complex information. To do so, we can pass multiple properties to our component! We'll use the same two steps we took to add the first prop.

First, add another prop to the component call: `<Hello name={"Nick"} />,` changes to `<Hello name={"Nick"} age={24} />`.

Update your `index.js` file to reflect this:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import Hello from './App.js';

ReactDOM.render(
  <Hello name={"Nick"} age={24} />,
  document.getElementById('root')
)
```

Now, in our component definition we have access to both values.  The second step is to change the `Hello` component class in `App.js` to use the age information!


```js
class Hello extends Component {
  render () {
    return (
      <div>
        <h1>Hello {this.props.name}!</h1>
        <p>You are {this.props.age} years old.</p>
      </div>
    )
  }
}
```


> Check it out! You should be able to browse to http://localhost:3000 to view this change!

## What about... multiple props passed from an object?

If we have many props, it might get difficult to keep track when we're passing everything in to render a component. A better practice is to organize values in some kind of object and then pass props to the component from that object. Let's see this strategy.

Currently, in `index.js`, we put Nick's name and age directly into the `ReactDOM.render` call. Instead, we'll create an object that holds Nick's name and age, making it clearer for other developers and easier to change in the future. In your `index.js file`, below the `import` statements, add this object definition:

``` js
var person = {
  personName: "Nick",
  personAge: 24
}
```

Next, we'll update what's passed into the component. Near the bottom of your `index.js`, modify the `ReactDOM.render()` call:

``` js
ReactDOM.render(
  <Hello
    name={person.personName}
    age={person.personAge}
  />,
  document.getElementById('root')
)
```

We don't have to change anything in `App.js`, because it's still receiving exactly the same values for exactly the same two props - `name` and `age`. We're just sending it those values in a slightly different way.

> Check it out! If you browse to http://localhost:3000 nothing should have changed.

> Try changing the values inside the `person` object without changing the `ReactDOM.render()` call. See how the page updates.

#### Multiple props from a more complex object

Since we're just pulling props out of an object, we can use any object we want. For example, we can nest an array inside it.

Let's say our user has some favorite animals. Update your object to include an array:

``` js
var person = {
  personName: "Nick",
  personAge: 24,
  favorites: [
    "capybaras",
    "Tigers",
    "Dinosaurs count!"
  ]
}
```

Now we can use this new information as a prop, just like normal. You could choose to pass a single element (`favorites[0]`) or the entire array.  We'll use the entire array so that the component can display _all_ a person's favorite animals. First, update your `ReactDOM.render()` call in `index.js`:

``` js
ReactDOM.render(
  <Hello
    name={person.personName}
    age={person.personAge}
    animals={person.favorites}
  />,
  document.getElementById('root')
)
```

If you check your application now, nothing has changed. Remember, a component class will just ignore any props it receives that it doesn't use. But, we want to use the favorite animals! So, second, update your `Hello` class `render` method in `App.js`:

```html
<div>
  <h1>Hello {this.props.name}!</h1>
  <p>You are {this.props.age} years old.</p>
  <p>You love: {this.props.animals}</p>
</div>
```

If you check the page now, you'll see React prints the entire array, as that's what was passed in. If we wanted to include all the animals clearly, we could fix the spacing. Instead, to review some syntax, let's just modify the code to render the first value.

```html
<div>
  <h1>Hello {this.props.name}!</h1>
  <p>You are {this.props.age} years old.</p>
  <p>You love: {this.props.animals[0]}</p>
</div>
```

Check it out!

*[Read more about using props in JSX, if you'd like!](https://facebook.github.io/react/docs/jsx-in-depth.html) This link is also in the Further Reading page at the end of the React module, under the Facebook documentation.*