<img src="https://i.imgur.com/ser5chI.png">

# Unit 2 - Project Assessment

## Introduction (By Instructor)

This **Introduction** section will be read in class by the instructor.

Students are to have their laptops closed during this intro period.

Students will be self-directed beginning with the **Instructions** section below.

### GOAL

The goal of this project assessment is to gauge your ability to develop a minimal NodeJS web application using the Express framework.

### OVERALL APPLICATION REQUIREMENTS

The requirements of the application are detailed in STEP 3 below, however, in brief, the application:

- Will obtain an array of **post** objects by making a request from the backend (Node app) to a third-party API endpoint (provided).
- Has only two separate views to be rendered using the EJS view engine.
- Views are to render a "nav" at the top of the page containing **Home** and **All Posts** links that provide navigation between the two views when clicked.
- Does not have data persistence and thus will not require: a connection to a database; any models to be defined, etc.

### PROCESS

This assessment is an **individual** assignment - no collaboration please.

The good news is that it's "open book" - you may reference anything on your computer, Google, use notes, refer to class lessons/labs, etc.

It is estimated that this assessment will take 60 to 90 minutes to complete, however, you will have up to 3 hours to complete the assessment should you need it - no submittal will be accepted after the allotted 3 hours. 

Words of advice: **Do not over-think this assignment!**

## Instructions

Please follow the following steps in order:

- **STEP 1 - Prepare**
- **STEP 2 - Set up the Project**
- **STEP 3 - Implement the App's Requirements**
- **STEP 4 - Deploy**

## Assessment Steps to Complete

### STEP 1 - Prepare

- Briefly read through the rest of this assignment to better understand what is required before starting to code.
- In Terminal, move to a directory that is not within an existing project or repo. You will be creating a directory and repo for this assessment.

### STEP 2 - Set up the Project

- Create a folder named `project-3-assessment`.
- Initialize a git repository in that folder.
- Create a new `server.js` file in the folder.
- Initialize a new npm project in the folder.
- Install the node modules you will need (you won't need a database for this.)
- Open the project in VS Code.
- Require all necessary node modules in the server.
- Configure all necessary Express middleware.
- Create a basic root route for initial testing (use `res.send()` initially.)
- Set the app to listen on port 3000 ( `process.env.PORT || 3000` )
- Start the app using `nodemon` and browse to `localhost:3000` - and you're on your way!

### STEP 3 - Implement the App's Requirements

#### Step 3.1 - The Navigation Bar

Both views should display two links at the top of the page like this:

<img src="https://i.imgur.com/GGkAv3I.png">

- Clicking **HOME** will return to the application's root route (`/`).
- Clicking **ALL POSTS** will browse to the path of `/posts`.

#### Step 3.2 - Implement the ROOT Route (`/`)

Upon typing `localhost:3000` or clicking the **HOME** link, the app should display a simple landing page that looks like this:

<img src="https://i.imgur.com/vHpWt75.png">

#### Step 3.3 - Implement the `/posts` Route

Upon clicking the **ALL POSTS** link, the app should return a view that displays all _post_ objects returned by an API endpoint (see below) like this:

<img src="https://i.imgur.com/GY61bmq.png">

The _post_ objects are being displayed in the page using an HTML `<table>`.

##### API Endpoint

- The _posts_ data displayed in the **ALL POSTS** view is obtained by making a request from the server to the following third-party API endpoint:<br>`https://jsonplaceholder.typicode.com/posts`

> Reminder: Making an HTTP request from the server is simplified by installing another NPM module...

> Yet Another Reminder: The "body" returned by the aforementioned module to make the HTTP requests is a string that needs to be parsed before being passed to the view.

### STEP 4 - Deploy

Follow these steps to deploy your app:

- Create the first commit: `$ git add -A` then `$ git commit -m "Initial commit"`
- Create a remote link to Heroku: `$ heroku create`
- Deploy: `$ git push heroku master`
- Test the application: `$ heroku open`

> It may take a minute for the application to become functional on Heroku.

**Slack the deployed app's link to your instructors - congrats, you are done!**
