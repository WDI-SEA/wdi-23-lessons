# MERN Code-Along with Context API

## Build the back-end API

1. Create new directory `my-fish` and go into it.
2. Run `npm init` to initialize the node app.
3. Install node modules: `express mongoose`
4. Touch `server.js` in project root.
5. `mkdir routes`
6. `mkdir config`
7. `mkdir models`
8. `touch config/database.js`
9. `touch routes/api.js`
10. `touch models/fish.js`
11. Open the directory in your code editor.
12. Add the database config code.
13. Create the Fish model with two fields: `name` and `breed`
14. Fill out server.js
15. Write API routes in `routes/api.js`. Use anonymous functions for the controllers in the same file.
16. Add start script to package.json
17. Test server
18. Insert data manually via mongo CLI:
```bash
> use myFish
switched to db myFish
> db.fish.insert([
... {'name': 'Wanda', 'breed': 'Angelfish'},
... {'name': 'Charlie', 'breed': 'Tuna'},
... {'name': 'Baby', 'breed': 'Shark'}])
... {'name': 'Mama', 'breed': 'Shark'}])
... {'name': 'Grandpa', 'breed': 'Shark'}])
```
19. Test api routes with Postman.

## Build the React app

1. Run `create-react-app client` in the project root to install the React app in a `client` subdirectory of the project.
2. Add the proxy line to package.json.
3. Clean unnecessary code out of App.js.
4. Create components directory.
5. Create components `Welcome, FishPanel, FishList, FishDetail, Fish`
6. Add styles to App.css
7. Layout basic JSX for whole app (static)
8. Test app.
9. Introduce context api (lecture)
10. Create the `FishContext` in components
11. Import it into App.js
12. Wrap the App's JSX in `FishContext.Provider`.
13. Set up the state in App.js. Add static test data.
14. Wrap the `FishList` in a `FishContext.Consumer`, add in mapping for `Fish` component.
15. Test
16. Wrap the `FishDetail` in the context consumer
17. Test
18. Add componentDidMount and API call to App.js
19. Test
20. Add clickHandler function to App
21. Make it an arrow function and add it to state
22. Wrap Fish in a context consumer
23. Assign onClick to Fish handleClick from context
24. Test
25. Clean up.