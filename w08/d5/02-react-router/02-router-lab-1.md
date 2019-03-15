# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) React Router Recap and Exercise

## Recap
What have we learned so far?
* Single Page Applications have specific URLs that are routed to display different content.
* React Router is a third-party library that we can install and use with React.
* Since React Router isn't built in to React, we must import its components.
* React Router makes it easy for us to route URLs to components.
* React Router automatically manipulates modern browser history mechanics.

Now let's put that to the test!

## You Do: Implement Router

Let's build a blog project.

Your blog needs to have five pages:
- Homepage: A main welcome page
- Main blog: A page displaying a few blog posts
- Favorite movie: Display your favorite movie and a bit about it
- Favorite food: Show your favorite food and tell us about it
- About page: A page about you and your skills

Tasks:

- Each page is a component - we're learning to use React Router here!
- Create a navigation menu of list items that Route to each page.
- The main App should be a class-based component so it can hold state
  - The main App's state should hold an array of `post` objects, each with a title and a body, both strings.
- The rest of the components should be functional.
- The main blog page should be rendered so that you can pass props (the posts) into it. Then render each post nicely.

## Don't Forgets

* Do you have `react-router-dom` installed for this project?

* Here is how to render a component with `props` inside of a `<Route>` element:

```js
<Route path="/blog" render={ () => <Blog posts={this.props.posts} /> }/>
```
