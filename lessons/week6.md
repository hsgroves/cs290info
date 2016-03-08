##Models, Controllers, and Views
Start by writing out some sentences that describe the most common actions in your system. For the in-class blog application demo, here are some examples:
  1. Users can write posts
  2. Users can delete their posts
  3. Users can categorize their posts with multiple tags
  4. Admin users can edit and delete any user's posts
  
From there, you'll want to start thinking about your app in terms of models, views, and controllers.
  - **Models:** Models are the entities within your system. The main models of your app should correspond as closely to real-life things as possible. The common nouns in the sentences you wrote are usually good candidates for models. From above: we have a `User` model, a `Post` model, and today/next class we'll add a `Tag` model.
  - **Views:** Views are the pages that get sent to the user's web browser. They are HTML pages that the user sees, and they normally contain text, images, links, and forms that the user interacts with.
  - **Controllers:** Controllers are the Ruby functions that are executed when a browser makes a web request. That might sound complicated, but they serve to connect the user's View with the predefined Models in your system. For example, when a user types the URL `myapp.com/posts`, the Posts controller's `index` function is executed. It retrieves all the Post models from the database, compiles them together with the corresponding .html.erb file, and returns the resulting HTML to the user's browser.

##Connecting Users and Posts: Many-to-One Relationships
So far, we've added users and an admin interface to our system... but we haven't done anything to connect a blog post with the user who made it. Let's do that now!

Each `Post` will correspond to exactly one `User` instance, but one `User` can make as many posts as they'd like. When two models are related like this, it's referred to as a "Many-to-One" or "One-to-Many" relationship. Rails uses `has_many` and `belongs_to` to describe this - applied to our project, a `User` `has_many` `Posts`, and a `Post` `belongs_to` a `User`.

In the database, we'll need to store a `user_id` for each Post. Let's create and apply a migration for that change:

```
bundle exec rails g migration AddUserRefToPosts user:references
bundle exec rake db:migrate
```

In `app/models/user.rb`, add the following:
```
has_many :posts
```

In `app/models/post.rb`, add the following:
```
belongs_to :user
```
We want to make sure that each `Post` is connected to a `User` - we aren't going to allow people who aren't signed in under a user account to make posts. To ensure that a Post model never gets saved without a corresponding User, we'll add a *validation* to the `Post` model. Under `belongs_to :user`, add the following:
```
validates :user, presence: true
```

This is far from the only kind of validation that can be done on different attributes of your model, but it's a good place to start - by making sure that in order to save your model to the application's database, it has all of the properties that you want it to. Let's also make sure that `Posts` can't be created without a `title` and `body`:
```
validates :title, presence: true
validates :body, presence: true
```
A complete list of Rails validations can be found [here](http://guides.rubyonrails.org/active_record_validations.html).

Now, we need to make sure that when a `Post` is created, it corresponds to the `User` who made it. In `app/controllers/posts_controller.rb`, we see all of our user-facing actions (the actions that are *not* handled through the admin dashboard).

Let's first make sure our `PostsController` requires a user to be logged in before they do anything except view posts. In `app/controllers/posts_controller.rb`, after line 2:
```
  before_action :authenticate_user!, only: [:new, :create, :edit, :update, :destroy]
```
`:authenticate_user!` is the name of the method that Devise gives us to check whether or not a user is logged in.

Great! Now, there are three actions that have to to with a User's specific posts: `create`, which will need to save a `Post` along with the user that created it, `update`, which should only successfully update a `Post` if the current user is the one that created it, and `destroy`, which should only allow a user to delete their own post. We're going to add a few things to the controller methods to associate the post that we're dealing with with the current user.

Replace line 28:
```
    @post = current_user.posts.new(post_params)
```

After line 43:
```
    return head(:forbidden) unless @post.user == current_user
```

After line 59:
```
    return head(:forbidden) unless @post.user == current_user
```

###Restricting which Users can do what to which Posts
Great! But we still see the links to do everything (create, edit, update, etc) for all posts. Let's fix that.

###Viewing Posts in the ActiveAdmin Dashboard
We've got a dashboard in ActiveAdmin, but it doesn't actually show our posts - that's because we haven't told ActiveAdmin which models it should show. To do that, we'll run the ActiveAdmin resource generator:

```
bundle exec rails generate active_admin:resource Post
```

This created `app/admin/post.rb`. Take a look - it's pretty empty, except some comments about "permitted params". Earlier versions of Rails allowed for a security hole in accepting parameters, if not set up properly; Rails 4 introduced "strong parameters," or a parameter whitelist, to allow only expected parameters to be passed to any given controller function. So we'll follow the suggestion, and add some permitted params to our Admin post model:

```
permit_params :title, :body
```

Now, we're able to create and edit any posts we'd like in the Admin section.

##Making Tags
Let's run a quick generator to make a Tag model. `Tags` are pretty simple - they're just a string that we're later going to associate with some `Posts`.

In your terminal:
```
bundle exec rails generate scaffold Tag title:string
```

##Connecting Posts and Tags: Many-to-Many Relationships
`Posts` aren't related to `Tags` in the same way that `Posts` are related to `Users`. A `Post` can only be associated with one `User`, but a `Post` could have many `Tags`. Likewise, we might want to know how many `Posts` are associated with a given `Tag` - so a `Tag` can't just be associated with one `Post`. We call this kind of relationship a "Many-to-Many" relationship. In Rails, there are two ways of doing this: with a `has_and_belongs_to_many` or a `has_many, :through`.

`has_and_belongs_to_many` should be used when no extra information is expected to be stored about the relationship. `has_many, :through` is more robust and usually better to use, so we're going to start with that. Even if you can't immediately think of any extra information about the relationship that needs storing, it's much easier to add later with a `has_many, :through` relationship.

More to come! 
