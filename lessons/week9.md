##Announcements

  * My office hour (Tuesdays after class until 7pm) will be in the CS lab from now on.
  * Next Tuesday, 4/12, is research day. We will not have class, but I will be on campus for office hours from 2:30-6pm.
  * *If* you guys think it's useful, I'll come to campus this Thursday, 4/7 for office hours from 6:30-7:45.
  * Go over schedule for the rest of the course

##How Rails handles dynamic HTML
Last week we discussed how HTML works - it's a way of tagging your text. On a static website, where the information doesn't change, that's a pretty simple concept. But in an interactive web app, some extra steps are required to put dynamic information, like the post you're currently viewing, into HTML form. Usually, the web server will compile variables together with an HTML template to form raw HTML, which is then returned to the client's web browser.

In Rails, ERB (*E*mbedded *R*u*b*y) is used by default (*). ERB allows you to insert Ruby code into HTML. When Rails gets a web request, it first executes the corresponding controller method. Any instance variables defined in that function (variables beginning with an `@` sign) are made available to the ERB template. Rails then executes the Ruby code within the `.html.erb` file, producing a true `.html` file to send back to the end user.

(*) - Other templating languages exist and can be used within Rails instead of ERB. The main differences are syntactical; the process works in roughly the same way for all of them. Other web application frameworks (Django, AngularJS etc) approach templating in different ways.

##Customizing your app's HTML & CSS
It's pretty common to have one "layout" or theme that looks the same across all pages in a website or app. Making a single page look nice is one task - but keeping all of the pages in an entire website looking the same is another.

You can imagine that if you had to copy and paste the common HTML elements in your layout across every single page, making site-wide updates to your layout would be very difficult. Rails addresses this through **partials**. Take a look at `/app/views/layouts/application.html.erb`. This default layout is rendered for every page on your site, unless you specify otherwise. We won't go over specifying otherwise in class, but if you feel you need to do so in your final project, shoot me a Slack message and I'll be happy to help!

Now, take a look at `/app/views/posts/index.html.erb`. Like I mentioned last class, this is only *part* of an HTML page, that gets inserted into `/app/views/layouts/application.html.erb` when the `PostsController#index` action returns. 

So in order to change the layout and general look and feel of our app, we'll change `/app/views/layouts/application.html.erb`. To change the way a specific page or view displays, find the corresponding partial and edit that.

##Updating the layout
In this folder, take a look at the `sample-layout.html` file. We're going to copy & paste that into our  `/app/views/layouts/application.html.erb`. HTML is nothing without CSS - so let's copy & paste `sample-css.css` into `/assets/stylesheets/application.css`.

## Deleting Scaffold CSS
When we use `rails g scaffold ....` to create models, a SCSS file called `scaffolds.scss` is created. Unless you like the look of things with this file in place, you should delete it and create your own styles.

##SCSS
You've probably noticed that most of the CSS files in our Rails app have a different extension - `.scss`. This is the extension for Sass, a CSS preprocessor that compiles to CSS. If you write syntactically correct CSS in any `.scss` file, that's fine! It'll handle it well.

Notice that `application.css` does *not* have the `.scss` extention. If you want to write Sass in this file, you need to add the `.scss` extension.

Sass does offer a number of *very* convenient shortcuts to make writing CSS less painful. Take the following code:
```
.menu {
  width: 19%;
  margin-right: 2%;
  float: left;
  text-align: right;
}

.menu ul.nav-links {
  list-style: none;
  margin: 0;
  padding: 0;
  margin-top: 10px;
}

.menu ul.nav-links li {
  margin-bottom: 10px;
}

.menu ul.nav-links li:hover {
  border-right: 10px #FF8256 solid;
  padding-right: 5px;
  transition: all 500ms ease;
}
```

With Sass, we can write it like so:
```
.menu {
  width: 19%;
  margin-right: 2%;
  float: left;
  text-align: right;

  ul.nav-links {
    list-style: none;
    margin: 0;
    padding: 0;
    margin-top: 10px;
    
    li {
      margin-bottom: 10px;

      &:hover {
        border-right: 10px #FF8256 solid;
        padding-right: 5px;
        transition: all 500ms ease;
      }
    }
  }
}
```
Notice how child selectors are nested inside the parent's block, much like these elements will be nested in the corresponding HTML. Also notice the ampersand, which denotes that in CSS, the selector would immediately follow its parent, with no space

You can choose to write regular CSS or explore the capabilities of Sass (there are a lot more! [check out the docs here if you're interested](http://sass-lang.com/)).
