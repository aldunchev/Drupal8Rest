# Drupal 8 REST Services module - connect with express js

*Basic instruction for setup of Drupal 8 Restful services module and Express JS Node.js framework*

## Drupal 8 configuration

* Install Drupal 8 with standard profile;
* Enable modules:
    * RESTful web services;
* Set up a content type;
    * Title;
    * Body;
* Configure RESTful web services;
    * Create view - tick provide a REST export when creating;
    * On format use fields instead of content;
    * Add Title field;
    * Add body field;
    * There is button to the right "view rest export", which shows the JSON data;

## Nodejs and Express js configuration

* Install node js;
* Install express js - `sudo npm install -g express`;
* Hello world app:
    * `mkdir myapp`;
    * `cd myapp`;
    * `npm init;
    * `npm install express --save`;
    * `touch myapp.js`;
    * add 
        ```var express = require('express')
        var app = express()

        app.get('/', function (req, res) {
          res.send('Hello World!')
        })

        app.listen(3000, function () {
          console.log('Example app listening on port 3000!')
        })```
    in myapp.js;
    * `node myapp.js`;
    * Open the browser: http://localhost:3000
    
## Better is to use the express generator

* `sudo npm install -g express-generator`;
* Create the project with twig template engine;
* `express --view=twig BlogExpress`;
* `cd BlogExpress`;
* `npm install`;
* `DEBUG=:BlogExpress* npm start`;
* open http://localhost:3000;

## Connect Drupal with express app;

* Install request module: `sudo npm install request --save`;
* Create the request to Drupal:
  * In app.js add:
  ```
    var request = require('request');
    var blogsUrl = "http://drupal8.local/blogs";
  ```
  * At the bottom of app.js add:
  ```
    request(
        {
            url: blogsUrl,
            json: true
        }, 
        function (error, response, body) {
            if (error) {
                console.log(error);
                return;
            }

            if (response.statusCode === 200) {
                app.locals.blogList = body;
            }
        }
    );
  ```

* Create the route and the template;
  * Add `app.use('/blogs', blogs);` in app.js;
  * Create a file `blogs.js` in routes and add:
    ```
    var express = require('express');
    var router = express.Router();

    /* GET blogs page. */
    router.get('/', function(req, res, next) {
      res.render('blogs', { title: 'Blogs', blogs: req.app.locals.blogList });
    });

    module.exports = router;
    ```
    * Create a file `blogs.twig` in views and add:
    ```
    {% extends 'layout.twig' %}

    {% block body %}
        <h1>{{title}}</h1>
        <ul class="blogs">
            {% for blog in blogs %}
                <li class="blogs__item">
                    <h2>{{blog.title}}</h2>
                    {{blog.body}}
                </li>
            {% endfor %}
        </ul>
    {% endblock %}
    ```
* View the blogs page;
* Restart the web server: `ctrl + c` and `DEBUG=:BlogExpress* npm start`;

