###Viewing Posts in the ActiveAdmin Dashboard
We've got this sweet dashboard now, but it doesn't actually show our posts - that's because we haven't told ActiveAdmin which models it should show. To do that, we'll run the ActiveAdmin resource generator:

```
bundle exec rails generate active_admin:resource Post
```

This created `app/admin/post.rb`. Take a look - it's pretty empty, except some comments about "permitted params". Earlier versions of Rails allowed for a security hole in accepting parameters, if not set up properly; Rails 4 introduced "strong parameters," or a parameter whitelist, to allow only expected parameters to be passed to any given controller function. So we'll follow the suggestion, and add some permitted params to our Admin post model:

```
permit_params :title, :body, :username
```

##Fixing the "Log Out" link on our Dashboard

##Connecting Users and Posts
So far, we've added users and an admin interface to our system... but we haven't done anything to connect a blog post with the user who made it. Let's do that now!

First, in order to connect the two models, we'll need to store a `user_id` for each Post. Let's create and apply a migration for that change:

```
bundle exec rails g migration AddUserRefToPosts user:references
bundle exec rake db:migrate
```

In `app/models/post.rb`, add the following:
```
belongs_to :user
```

In `app/models/user.rb`, add the following:
```
has_many :posts
```

...

##Making sure regular users can only edit/remove their own posts
