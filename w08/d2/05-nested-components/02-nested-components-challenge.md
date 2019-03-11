# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) You Do: Add Nested Authors To Blog

This blog is great so far! Now, our stegosaurus is actually collaborating more on the blog, so each post has multiple authors. Let's set that up. Besides Stealthy Stegosaurus, we have authors named Tiny T-Rex and Ivory Iguanodon.

### Steps

* From the `index.js`, modify the `render` method to pass the `Post` component a prop called `allAuthors`, which will be the `authors` array.

* Create an `Author` component that renders "Written by ", followed by an author.

* Amend your `Post` component's `render` method first and create a variable, `authors`, which is equal to the return value of generating multiple `<Author />` elements.

> Hint: This variable will contain three calls to the `Author` component.

* Make sure to pass in the `allAuthors` body as an argument to each `Author` component.

* Render the `authors` somewhere inside the UI for a `Post`.

> Hint: Remember that whenever you write Javascript expressions inside of JSX, you need to surround them with single brackets (`{}`). Refer to the advanced note on the previous page...

<i><strong>Advanced challenge:</strong> If you'd like, you can use JavaScript's array `map` method in `Post`'s `render` method to avoid having to hard-code all your `Author`s. Read more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) and [here](http://cryto.net/~joepie91/blog/2015/05/04/functional-programming-in-javascript-map-filter-reduce/).
> <strong>Hint for using Map:</strong> If you're using `map`, you should only have to return one `<MyPost />` inside of `map`.</i>

## Solution

Your solution should look as follows:

![Solution for Project](../images/nestedsolution.png)