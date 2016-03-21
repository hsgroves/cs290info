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
