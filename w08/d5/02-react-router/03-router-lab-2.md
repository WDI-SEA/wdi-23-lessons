# Bean Bank

You will be making an app for viewing details about specific coffee roasts.

## Components

* Home page: Welcomes people to the site
* Bean List page: Shows all the beans and links to the details pages
* Bean Detail page: Shows the details of each bean

## Starter Data

This bean data should be in the state of your App. The rest of the components can be functional. Feel free to add more data:

```js
[
  {
    name: 'Sumatra',
    roast: 'dark',
    notes: 'Full-bodied with floral notes'
  },
  {
    name: 'Breakfast Blend',
    roast: 'medium',
    notes: 'Well-balanced and earthy'
  },
  {
    name: 'Italian Roast',
    roast: 'medium-dark',
    notes: 'Rich with chocolate notes'
  },
  {
    name: 'French Roast',
    roast: 'dark',
    notes: 'Deep and robust'
  },
  {
    name: 'Espresso',
    roast: 'dark',
    notes: 'Strong and bitter'
  }
]
```

The Bean List component should be passed this list of data from the App as a prop. Each entry on that page should be the name of the bean which should `Link` to the Bean Detail component and pass in that bean object as a prop so that the details can be displayed.

Be sure to give it your own sense of flair!