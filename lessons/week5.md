##[Ruby Topics](https://github.com/hsgroves/cs290info/blob/master/ruby_topics.md)

##Devise and ActiveAdmin
[Devise](https://github.com/plataformatec/devise) is a free, open source Ruby gem for Rails authentication (user login).

[ActiveAdmin](https://github.com/activeadmin/activeadmin) is a free, open-source Ruby gem that contains a Rails engine (a mini Rails app that runs within another Rails app). It gives you an *extremely* robust administrative interface with minimal setup.

##Devise Install
You can't have a user admin interface without first having users! So let's make that happen.

###Setting a root route
Remember, routes map an HTTP request to a controller function. When you set the "root" route, you set the configuration for the landing page of your app (with no extra URI path).

In routes.rb:
`root to:posts#index`
This means that when we visit C9 URL HERE.io/, we will be taken to the index action of our Post model.

###Include & install the Devise gem
Add a line in your Gemfile:
`gem 'devise'`

Then, run `bundle install` in your terminal.

###Generate configuration, User model, and views
In your terminal, run the following:

```
bundle exec rails generate devise:install  # generates configuration files (we'll edit these later)
```

That prompts you to add some configuration (!!!!!!!!!!)

```
bundle exec rails generate devise user     # generates a user model
```

Just like after we used `rails generate scaffold` to create our Post model, we now need to apply the schema migration generated by the second line above.

```
bundle exec rake db:migrate
```

Now that we've got a User model, we can generate HTML views for it (so that a real live user can create an account).

```
bundle exec rails generate devise:views users
```

##ActiveAdmin Install
And now that we have users, let's give certain users admin rights, so that they can moderate blog posts and future features (like comments and tags).

###Include & install the ActiveAdmin gem
Add a line in your Gemfile:
`gem 'activeadmin', github: 'activeadmin'`

Notice that we're specifying that we want to install the specific version of ActiveAdmin that . We're doing this because previous major releases of ActiveAdmin are not compatible with Rails 4.

Save the Gemfile and run `bundle install` in your terminal.

Then, run `bundle install` in your terminal.

###Run generator
We already have a User model. It makes sense that blog administrators might start out as regular users, and should have all capabilities that regular users have, too. So we're going to run the ActiveAdmin generator, and tell it to use our existing User model to configure for ActiveAdmin.

```
bundle exec rails g active_admin:install User
bundle exec rake db:migrate
bundle exec rails generate active_admin:resource User  # (?????????)
```

Once we start our server, we can visit `http://localhost:3000/admin` and log in with the default credentials:

- User: admin@example.com
- Password: password

##Connecting Users and Posts
So far, we've added users and an admin interface to our system... but we haven't done anything to connect a blog post with the user who made it. Let's do that now!

In `app/models/post.rb`, add the following:
```
belongs_to: user
```
In `app/models/user.rb`, add the following:
```
has_many: user
```