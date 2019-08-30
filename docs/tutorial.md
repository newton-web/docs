---
id: tutorial
title: Tutorial
sidebar_label: Tutorial
---

This covers a basic tutorial to get a simple app up and running with Newton Web (Newton).
If you haven't already, see the "Getting Started" section to install the Newton CLI.

## Creating a Project
Once you have the Newton CLI installed, you can start your new project via:

`newton-admin startproject blogTutorial`

This will create all of the files you need to start your project.
Then, change your directory into your project and run `npm install`.
For example:

```bash
cd blogTutorial
npm install
```

Once you are ready, you can run `nodemon app.js` (or whatever you use to automatically restart your Express.js server).

You should now be able to go to `localhost:8000` and see your project running!

## Configuring your Home Route
By default, your route at `localhost:8000` is configured in `blogTutorial/main/routes.js`.
If you change the code in your `mainRouter`'s first `app.get` to configure your home route to go wherever you want.
Change the string in the `response.send` to "Welcome to my Blog".
The `mainRouter` will take care of all of the routing for your project.

## Creating an App
The best practice is to create an app for each table you will have in your database.
Let's create a new app called blogs (we aren't calling it posts due to potential confusion with POST requests). In your terminal, run:

`newton-admin startapp blogs`

Then, in your `main/settings.js`, add 'blogs' to your `apps` array.
This is necessary for future functionality, such at views and models.

Let's make it so that we can access routes within your blogs app by using Express router.
There will be three steps:
1. Create a controller in your `blogs/controllers.js`
2. Create a route that calls your controller in your `blogs/routes.js`
3. Include the `blogs/routes.js` routes in your `main/routes.js`

In your `blogs/controllers.js`, write the following code:

```javascript
module.exports = {
    index: (request, response) => {
        response.send("Hello World");
    }
}
```

Then, your `blogs/routes.js` should look like this:
```javascript
const router = require('express').Router();
const blogsController = require('./controllers');

//Your routes go here
router.get('/', blogsController.index);

module.exports = router;
```

And finally, add the routes to your `main/routes.js`:

```javascript
const settings = require('./settings');
const blogRoutes = require('../blogs/routes');

mainRouter = (app) => {
    app.get('/', (request, response)=>{
        response.send(`Welcome to Newton! You are running your server at ${settings.port}`);
    });
    //Put your other routes/routers here
    app.use('/blogs', blogRoutes);
}

module.exports = mainRouter;

```

Go to `localhost:8000/blogs` and you should see "Hello World!".
Congratulations! You have just set up your first route.

## Add a View to your Controller
Within your 'blogs' app, create a 'views/blogs' folder, and a file called 'index.ejs' within the views directory.
If you plan on having view files with the same names in different apps, you must nest the files in another folder name.

Within your index.ejs, you can put whatever you want.
For example:

```html
<html lang="en" dir="ltr">
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
```

Then, in 'blogs/controllers.js', change your response to:

```javascript
module.exports = {
    index: (request, response) => {
        response.render("blogs/index");
    }
}
```

Now, we can include html templates in our Newton project.

## Models
Now, let's add models to our project.
Newton comes ready to go with MongoDB and Mongoose set up.
By default, Newton will use a MongoDB database with the same name as your project, and you can adjust this in your `main/settings.js` via the `database.databaseURL`.
Make sure you are running MongoDB in the background before beginning this part.

Next, let's create a simple model for our Blog Post in `blogs/models.js`;

```javascript
const mongoose = require('mongoose');

const BlogSchema = new mongoose.Schema({
    title: { type:String },
    content: { type: String }
}, {timestamp: true});

mongoose.model('Blog', BlogSchema);
```

In the `main/db.js`, Newton takes care of most of what you need for the collection creation.
Next, in your controller, let's create a new blog in our index method.
When we visit our index view, we will create a new blog with the title of "My new Blog" and content of "This is blog content".
If you want to manually check it in the command line in your database, you can.
However, let's create another method in our controller to query our database for all Blogs.
Our `blogs/controllers.js` should look like this now:

```javascript
const mongoose = require('mongoose');

module.exports = {
    index: (request, response) => {
        const Blog = mongoose.model('Blog');
        let blog = new Blog();
        blog.title = "My new Blog";
        blog.content = "This is blog content";
        blog.save();
        response.render("blogs/index");
    },
    list: (request, response) => {
        const Blog = mongoose.model('Blog');
        Blog.find({}, (err, blogs)=>{
            if(err){
                response.json(err);
            }else{
                let context = blogs;
                response.render("blogs/list", {blogs: context});
            }
        })
    }
}
```

We are using a new view now, `list.ejs`.
We will have to create a new file in our blogs/views/blogs called `list.ejs`, which will contain the following:

```html
<html lang="en" dir="ltr">
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <% for(let i = 0; i < blogs.length; i++){%>
            <p>Title: <%= blogs[i].title %></p>
            <p>Content: <%= blogs[i].content %></p>
        <% } %>
    </body>
</html>
```

This will loop through and display all of our blogs (only one so far if you have only visited the index method once).
Finally, we will need to create a route so that we can display our list controller.
Our `blogs/routes.js` will have another route added.

```javascript
const router = require('express').Router();
const blogsController = require('./controllers');

//Your routes go here
router.get('/', blogsController.index);
router.get('/list', blogsController.list);

module.exports = router;
```

If you visit `localhost:8000/blogs/list`, you should see your blog(s) that you have created.

The rest of the functionality, at this point, is really just applying Express.js and Mongoose, o you should refer to their documentation for further questions.
