# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) You Do: Film Exercise - Unidirectional Flow

## Your Mission

Stop any project you currently have running; let's go back to the film application that you've started. You can run the app with `npm start`.

You're almost finished! Now, you need to:

- Add films to a user's faves
- Filter the films the user is looking at

To do this, you'll need to lift your state upwards so that all of the data is more easily shared across components. Remember unidirectional flow - data is *only* going to go *down* the component tree! Find the highest level of the tree in which there are two or more *sibling* components that need information about the same state; the state should reside in the parent of those components so it can be passed downward to all of them.

![](http://bitmakerhq.s3.amazonaws.com/resources/react-film-library-component-hierarchy.png)

### Task 1: Add State to the Appropriate Components

#### Step 1: Add a constructor to `App.js`

In `App.js`, start by creating a constructor. Your constructor should call `super()`.

#### Step 2: Initialize the state object

Now, set two states:

1. `films`: initialize this key to hold a reference to `TMDB.films`
2. `current`: this key should start off as an empty object

#### Step 3: Pass state values to `FilmListing` and `FilmDetails` as props

Now that you have state stored on the `App` component, you want to pass those as props to your child components. Just change `App.js` for now:

- `FilmListing` should receive a `films` prop that now references the `App.js` `films` state.

- `FilmDetails` should receive a `film` prop that references the `App.js` `current` state.

Since you aren't doing anything with these props yet, nothing should change.

#### Step 4: Add a `Faves` state to `FilmListing.js`

In `FilmListing.js`, add a `faves` state, which is initialized as an empty array.


### Task 2: Move the `Fave` Event Handler up the Component Tree
When a user favorites a film, that information needs to be shared with the `FilmListing` component and all it's children in order to properly filter the list.

This isn't possible right now, because you're currently handling the favorite toggling of a film on the `Fave` component. The `Fave` component is at the bottom of the component hierarchy, and props and state only flow downward. Additionally, the `Fave` component doesn't even know which film is being favorited, so this isn't a great place to store a state for whether a film is a favorite.

Let's fix this:

#### Step 1: Remove the state setter in the `Fave` constructor

Take the `isFave` state out of the `Fave` constructor.

#### Step 2: Replace `setState` in `handleFaveClick` handler

Since you're no longer holding the state in the `Fave` component, you no longer want to set the `isFave` state in the `handleClick()` event handler.

Instead, assume that the parent component will pass a handler called `onFaveToggle` to you through the props object.

Change `handleClick` as follows:

```js
# /src/Fave.js

handleClick(e) {
  e.stopPropagation()
  console.log('Handling Fave click!')

  // Add this line. You'll call the function passed through props
  this.props.onFaveToggle()

  // Delete the `setState` line. You no longer track state here
  // this.setState({isFave: !this.state.isFave})
}
```

This way, when a user clicks, `onFaveToggle` will be called at a higher component level.

#### Step 3: Change `isFave` to be a prop rather than a state

You've taken the `isFave` state out of `Fave` and will be passing a prop called `isFave` instead. In the `Fave` component, replace `this.state.isFave` with `this.props.isFave`. You'll send that information down from a parent component that knows this info.

This is all you need to change in `Fave.js`! It will still check to see if the user has clicked the fave toggle button. The difference is that once the user clicks, instead of just changing the color of the fave icon, the `handleClick` function will instead call `onFaveToggle` to add/remove that film from the faves array in <code>App.js</code>.

You'll define `onFaveToggle` in a higher component.

#### Step 4: Define `handleFaveToggle` on the `FilmListing` component

The `Fave` component is expecting a prop, but one doesn't exist yet. Let's change that next.

You'll move the favorite toggle functionality all the way up to the `FilmListing` component - where the state for `filter` is.
- In the `FilmListing` component, create a `handleFaveToggle()` function. It doesn't need to do anything yet, but soon you will update the `faves` array when a film is favorited or unfavorited. The `handleFaveToggle` function should accept a film object as an argument (this will be the film that the user is toggling).

#### Step 5: Bind the handler to the component (skip if using arrow functions)

As you saw previously, you need to bind your custom component methods to ensure `this` refers to the component within the body of the method. *Remember that arrow functions do this for you!! This step is unecessary in ES6*

Add the following to the `FilmListing` component's constructor:

```js
this.handleFaveToggle = this.handleFaveToggle.bind(this)
```

#### Step 6: Clone the `faves` state

To recap, the `faves` state is going to hold the user's favorite films. Your goal is to, when the user clicks the icon to favorite or unfavorite a film, either add or remove the given film from the `faves` array.

To do this, you need to call `setState` and give it the updated array (you can't just update it directly; otherwise React won't know to re-render the components to reflect the changes). To accomplish this, you'll make a copy of the existing faves array, update it, then pass the copy to `setState`.

First, just make a copy. Inside `handleFaveToggle`, use the JavaScript [`slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) method to make a copy and store it in a `const` variable called `newFaves`. [`Why slice?`](https://stackoverflow.com/questions/7486085/copying-array-by-value-in-javascript)

#### Step 7: Find the index of the passed film in the `faves` array

Now underneath the slice, use the JavaScript [`indexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) method to store the position of the film in the array in a `const` variable called `filmIndex`.

Now, `filmIndex` will be an index value starting at `0`.

#### Step 8: Set up a conditional for adding or removing film from the `faves` array

If the film is found in the array, `indexOf()` will return an index value starting at `0`. Conversely, `indexOf()` will return `-1` if the element isn't found - if the film it's looking for is not currently in the `faves` array.

Since this `handleFaveToggle()` function is designed to change the array of the user's favorites film, there are two options.
* If the film is already in their favorites, then when the user clicks the button, they want to remove it from their favorites. You need to take it out of the `newFaves` array.
* If the film is not in their favorites, then when the user clicks the button, they want to add it to their favorites. You need to add it to the `newFaves` array.

Write a conditional statement with the two cases. When adding a film to `faves`, log out `Adding [FILM NAME] to faves...` and when removing a film from `newFaves`, log out `Removing [FILM NAME] from faves...`.

#### Step 9: Change whether the film is in `faves`

To remove a film that's already in the `newFaves` array, use the [`splice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) method.

To add a new film to the `newFaves` array, just [push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) it on to the end of the array.

#### Step 10: Use `setState` to update the state of `faves`

Now that you have updated the `faves` array, you need to call `setState` so React will re-render the appropriate components in the tree. If you called the copy of the `faves` array the same name (`faves`), then you can make this very succinct by using object literal shorthand. It should look like this:

```js
this.setState({faves})
```
Otherwise, if you called the copy `newfaves`, for example, you'll need to write out the entire key-value pair:

```js
this.setState({faves: newFaves})
```

#### Step 11: Pass the `handleFaveToggle` function to `FilmRow` through props

In the `FilmListing` component, you render one `FilmRow` component for each film in the `films` prop. You need to pass the `onFaveToggle` function down to each `FilmRow`.
 
<details>
  <summary>Your map function should look like this:</summary>
  <code> const allFilms = this.props.films.map((film) => {return ( < FilmRow film={film} onFaveToggle {this.handleFaveToggle} /> )}); </code>
</details>

#### Step 12: Pass the `onFaveToggle` function to `Fave` through props

Now each `FilmRow` component has an `onFaveToggle` prop, but ultimately it's the `Fave` component that will call it. Note that in order to call `onFaveToggle`, we'll need to pass in *which* film as a parameter.

In the `FilmRow` component's `render` method, pass the `onFaveToggle` to the `Fave` component. Be sure to wrap it in an anonymous arrow function so you can include the current film as a parameter.

<details>
  <summary>The call to the <code>Fave</code> component should look like this:</summary>
  <code><Fave onFaveToggle={() => {this.props.onFaveToggle(this.props.film)} } /></code>
</details>

#### Step 13: Pass `isFave` down from `FilmListing` through `FilmRow`

The `Fave` component is also expecting to receive a prop called `isFave`, so you need to pass `isFave` to the `Fave` component from `FilmRow`.

`FilmRow` doesn't know about the `faves` array, but its parent, `FilmListing`, does.

The `isFave` prop should be true or false depending on whether the film is in the `faves` array.

In `FilmListing`, when creating each `FilmRow`, pass a prop called `isFave` whose value uses the [`includes()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) method to determine if the film is in the faves array or not.

<details>
  <summary>The call to the <code>FilmRow</code> component should now look like this:</summary>
  <code>< FilmRow film={film} onFaveToggle={this.handleFaveToggle} isFave={this.state.faves.includes(film)}/></code>
</details>


Now `FilmRow` is getting the `isFave` prop, but it doesn't need it. It only needs to pass it along. In `FilmRow`, pass that prop through to `Fave`.

Now, `Fave` is getting the true or false boolean of whether a film is a favorite (`isFave`), as well as being passed the favorite toggle function (`onFaveToggle`).

`Fave` has everything it needs!

Look in your browser to see this working - the JavaScript console will log if something is added or removed from the user's favorites.

#### Step 14: Update `Faves` counter

Currently in the browser, the `faves` counter the user sees is always 0. You'll update the counter in the `FilmListing` to accurately show the number of faves in the array.

If you look at what's rendered in the `FilmListing` component, right now the faves counter is hardcoded to `0`. Replace that with the length of the `faves` array that is received through the props.

You now have favorites properly stored and available to all components, and you have a counter that accurately reflects that to the user.

Great job! Check it out in your browser.

### Task 3: Move the details event handler up component tree from `FilmRow`

In `FilmRow`, there's still the function to handle when a user clicks a row for more details. The `FilmDetails` component needs to know which movie to show details for though, so we'll need to move the `handleDetailsClick` to `App.js`, since it is the parent of both `FilmListing` AND `FilmDetails`. `handleDetailsClick` will change the `current` state.

* Move the `handleDetailsClick` definition to the `App` component.
* Pass `handleDetailsClick` to `FilmListing` as a prop.
* Pass `handleDetailsClick` to `FilmRow` as a prop.
* In the `onClick` of `FilmRow`, wrap the `handleDetailsClick` prop in an anonymous function so you can pass in the `film` prop.

For `handleDetailsClick` in the `App` component, just log to the console and set the `current` state to the passed film for now. You'll handle looking up film details later. Makesure you pass the `current` state as a `film` prop to the `FilmDetails` component.

### Task 4: Make the filter work on `FilmListing`

You have the `filter` state on `FilmListing`, but you still need to make it actually change the UI. You're not going to move the `filter` state because this filter only affects the `FilmListing`, not any other parts of the app.

Add a conditional in `FilmListing` so that if the `filter` state is set to `faves`, the listing only shows films in the faves array. Otherwise, it shows all films.

* In the `render` method, define a constant called `filmsToDisplay` and us a turnery statement to return all the films, or just the favorite films.
* Change your `allFilms` map function to be called on `filmsToDisplay` instead of `this.props.films`.


<details>
  <summary>Hint:</summary>
  <code>
    
    const filmsToDisplay = this.state.filter==="all" ? this.props.films : this.state.faves;
    
    const allFilms = filmsToDisplay.map((film) =>{
    
  </code>
</details>

Try it out - you should be able to add films to your favorites and view just your favorites list by clicking that tab.