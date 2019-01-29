# CSS Media Queries

## Learning Objectives

- Explain what is meant by Responsive Design
- Explain the use and benefit of Media Queries
- Use media queries to apply different sets of styles based on device width
- Use media queries in conjunction with CSS Grid to achieve responsive layouts.

## Background

Not that long ago building a successful online presence meant just ensuring that your website worked correctly in all the major desktop browsers. 

Fast forward to today and desktop browsing is rapidly being replaced by surfing on mobile devices. More than 71% of the US population owns a smartphone and roughly half of all web browsing happens on those devices.

* **195 million** tablet devices were sold in 2013.
* The number of active mobile devices and human beings crossed over somewhere around the [7.19 billion mark](http://www.independent.co.uk/life-style/gadgets-and-tech/news/there-are-officially-more-mobile-devices-than-people-in-the-world-9780518.html).
* New devices like watches and activity trackers are changing the game too.

### Examples of responsive sites:

- [Boston Globe](http://www.bostonglobe.com/)  
- [GA](https://generalassemb.ly/)

## Making Responsive Webpages

### The Viewport

When web developers started creating sites for mobile, they had problems with displaying pages that were initially created for desktops. They were too large! The quick fix was to scale pages to fit the screen, but we ended up getting results like this:

![No Viewport Meta Tag](https://developers.google.com/speed/docs/insights/images/viewport/iphone_no_viewport.jpg)

Yikes, that's hard to read on a small device. The solution is that we need to control the width and the zoom level of the page.

When creating sites for mobile (which you should almost always do), we want to control the page width and zoom level of the browser using the viewport tag:

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

Doing so will give us something like this:

![Viewport Meta Tag](https://developers.google.com/speed/docs/insights/images/viewport/iphone_viewport.jpg)

Much better! Pictures courtesy of [Google Developers](https://developers.google.com)

## Media Queries

Media queries are simply a way to conditionally apply styles based on the device the page is being displayed on.

We already know that if we do something like this:

```css
p {
  color: red;
}

p.blue_text {
  color: blue;
}
```

By default, all p tags will have red text, unless they have the class blue_text, in which case, the text will be blue. We can do a similar thing with media queries.

```css
p {
  color: blue;
}

@media screen and (min-width: 600px) {
  p {
    color: red;
  }
}
```

Now, all p tags will be red, until the screen size decreases to 600px, when they'll turn blue. While this isn't super useful, it does demonstrate how we can apply styles conditionally when we meet certain device measurements.

## Anatomy of a Media Query

The syntax is quite a bit different than normal CSS so let's examine what we see here:

```css
@media screen and (min-width: 600px)
```

Media queries always begin with `@media`. The second option you see there is the word `screen`. This tells the browser that we are interested in screen measurements. There are two others we could use instead of `screen`: `print` is used to apply styles when the document is being printed, and `speech` is used to apply styles for screen readers. We'll mostly be using `screen`. You then need the word `and` to connect the the first test (whether we are on a screen) to the second test (whether the screen has a minimum width of 600px.) That last test is always in parentheses and always takes the form of a CSS property-value pair.

You can chain multiple tests onto your media query. For example, you wanted to apply styles between a min-width of 600px and a max-width of 900px, your code would look like this:

```css
@media screen and (min-width: 600px) and (max-width: 900px) {
  /* some styles here... */
}
```

There are many, many bits of information about the environment that we can query. You can see a full list [here](https://www.w3schools.com/cssref/css3_pr_mediaquery.asp). However, for the most part, we will only be querying `min-width` and `max-width` from the `screen`.

## A More Useful Example...

Media queries are some of the most powerful things we can use to control the style of our website, so changing the color of a `<p>` element probably isn't a super compelling example.

A potentially more useful example would be to change the layout from a single-column mobile layout to a multi-column desktop layout as the screen width increases. Let's use Codepen.io to make a very simple HTML page:

```html
<header>header</header>
<aside>sidebar</aside>
<main>main</main>
<footer>footer</footer>
```

Codepen treats these as contained inside the body element directly. Now, let's define what our mobile experience will look like. It will be a single column so the CSS will look like this:

```css
body {
  min-height: 100vh;
  margin: 0;
  display: grid;
  grid-template-rows: 80px 80px 1fr 80px;
  grid-template-columns: 1fr;
  grid-template-areas: "header"
                       "sidebar"
                       "main"
                       "footer";
}
header {
  background-color: red;
  grid-area: header;
}
aside {
  background-color: blue;
  grid-area: sidebar;
}
main {
  background-color: white;
  grid-area: main;
  min-height: 100px;
}
footer {
  background-color: gray;
  grid-area: footer;
}
```

So we have defined a grid with four rows and one column, a pretty standard layout for mobile. I gave the `main` section a minimum height so that it doesn't vanish on us while we are testing.

Now, using a media query, we can add a test for the times when the device width is as wide as on a laptop or desktop. When this test is true, the styles inside the media query will be applied and will override our mobile styles:

```css
@media screen and (min-width: 900px) {
  body {
    grid-template-rows: 80px 1fr 80px;
    grid-template-columns: 200px 1fr;
    grid-template-areas: "header header"
                         "sidebar main"
                         "footer footer";
  }
}
```

The browser will still first apply all the styles that show up earlier in our CSS file. Then it encounters the media query. Think of this block as a conditional that first tests to see if the screen has a minimum width of 900px. If that is true, all the styles inside the block are applied. If it is not true, the enclosed styles are skipped.

The styles we have inside the media query change the layout to have 3 rows and 2 columns and then reposition the grid areas within the grid. We can view the changes by resizing our browser width.

## A note about breakpoints

A media query that we set for a specific width is known as a breakpoint. It is the point at which one set of styles stops being applied and another set takes over. You may be asking yourself, when do I make the styles change? Do I put in the widths of every device that exists to accomodate them all?

No, because that would be nearly impossible to maintain. You don't even need to plan for the major screen sizes. Rather, you should let your design inform where the breakpoints should be. All modern designs should start with mobile styling first. Make it look good on mobile and then start expanding your page. How will you know when to add a breakpoint? You should add your first breakpoint at the width where things start looking crappy. If you widen your browser and eventually you feel that the single-column mobile layout looks overwhelmed by the amount of whitespace around it, then you should add a breakpoint right before the point where it starts detracting from the aesthetic. At this first breakpoint, change your styles to take advantage of the increased width. Maybe move some stuff into additional columns. Then repeat the process: widen the window until it no longer looks good. Then add another breakpoint to rearrange things until you like the look. Rinse. Repeat until you have dealt with the largest width you will need to accomodate (today this is usually a 4K ultra-HD television).

## Conclusion

Media queries are at the heart of every responsive design solution. While there are a few good CSS frameworks that include grid systems that we can use for laying out a responsive page (Bootstrap, Materialize, Skeleton, etc.), they are not a substitution for knowing how to use media queries well. While these frameworks work generally well for many, many web sites, eventually in your career you will find you need the control that only media queries provide.

## Additional Reading

* [7 Habits of Highly Effective Media Queries](http://bradfrost.com/blog/post/7-habits-of-highly-effective-media-queries/)
* [Media Queries for Standard Devices](https://css-tricks.com/snippets/css/media-queries-for-standard-devices/)
* [Brad Frost - Navigation Patterns for Responsive Design](http://bradfrost.com/blog/web/complex-navigation-patterns-for-responsive-design/)
* [Using Media Queries (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries#scan)

# Your Turn To Code:

Now you get to practice! You will be mocking up the home page of [microsoft.com](https://www.microsoft.com/en-us/). This page is responsive so take a moment examine it using the browser's responsive tools. You can also simply drag the edge of your browser window to narrow the viewport and see the responsiveness.

A few notes on the design:
1. At full laptop width, it is a 4-column page.
2. At medium width, it becomes a 2-column layout and the right two columns stack under the left two.
3. At mobile sizes, it becomes a single column.

## Mobile First!

Don't think about this page as a desktop page and then alter styles as the width decreases down to mobile. Mobile should be the target for all of your default styles and then you should add breakpoints for when your minimum width is a higher value (larger screen).

So in your stylesheet, you should start with all of your mobile styling, then add a breakpoint for your medium style, and lastly add a breakpoint for your full-width, 4-column version.

## Stay Focused!

This is an exercise in media queries and layouts. The main goal is to replicate the layout and responsiveness of the MS home page so please do not worry about duplicating images or text. Use placeholder images and lorem ipsum text where-ever it lets you focus on the layout and responsiveness. You do not need to reproduce all the links in the footer.

There are *several* ways this page responds to changes in width. Focus on getting the columns working properly.

## For a BONUS: Try making some of the other things responsive:
* The header bar changes somewhere between the 2-column and 1-column breakpoint. Add another media query breakpoint to deal with this.
* The large feature items (the images that span all columns) seem to have a margin that changes. Experiment to see where this changes and implement it.
* Finally, the feature items actually seem to change images when going down to single column. You'll notice they have a more landscape orientation until mobile sizes and then they become portrait. See if you can come up with a solution for this - changing the images based on media queries.

