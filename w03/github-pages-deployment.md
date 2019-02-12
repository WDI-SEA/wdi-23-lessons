<img src="https://i.imgur.com/Gd2y6TU.jpg" width="900">

# Deploying Your Static App to GitHub:Pages

## Intro

This quick walk-thru will show you how easy it is to deploy your static web application so that it can be accessed by anyone with Internet access, anywhere in world!

Any GitHub repository containing an _index.html_ and other static assets can become a website by simply using Github's deployment feature.

## What is a _static_ application?

A _static_ application is an application that consists solely of **static assets**.

Static assets are files that are not "processed" by the web server in any way, they are simply delivered by the web server as they are requested by the browser.

Typical **static assets** include:
	
- **html** files
- **css** stylesheets
- **javascript** files
- **image** files

## Deploying with ghpages

#### Ensure your project is ready to deploy

Make sure that your project is working - it does not make sense to deploy a broken app!

Next, make sure that all changes to your code are committed to _master_ in your local repository.

Once the working project's source is fully committed, go ahead and push the latest version to _origin_:<br>`$ git push origin master`

#### Go to `Settings` for your Github repo

<img src="https://i.imgur.com/yl3GCyj.png?1">

#### Scroll down to the `Github Pages` section

<img src="https://i.imgur.com/TdcZQn9.png">

#### Select `master branch` from the Source drop-down

<img src="https://i.imgur.com/YGNdcuI.png">

Finally, click Save to save your changes. The page will refresh and the Github Pages section will have a temporary message about your site being ready to publish. Wait for about a minute and then refresh the page.

You should now see this message in the Github Pages section, indicating that your site has been deployed:

<img src="https://i.imgur.com/ZO7A3y2.png">

## Browsing to the WebSite

Your website should now be up and running at the following URL:<br>`https://<user-name>.github.io/<repo-name>/`

Be sure to substitute **your GitHub user-name** for `<user-name>` and **your project's repo name** for `<repo-name>`.

>Note: It can take up to 15 minutes for the app to be available, but usually it takes less than a minute. Just wait and refresh the page until you see the green success message.

## Updating your Application's Deployment

If you need to make further changes to your published project, simply make the changes locally, make sure to `add` and `commit` them, then `push` the latest changes up to your master branch on Github. Github will then automatically republish your site with the changes you just pushed. (This republishing step can take up to a few moments, just as before.)

Happy Coding!
