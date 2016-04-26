##ActiveRecord Query Interface

Highly recommended reading: [ActiveRecord Querying docs](http://guides.rubyonrails.org/active_record_querying.html)

Many navigation and search functionalities in applications can be boiled down to database queries.

A quick re-introduction of how our Post, Tag, and Categorization models are related:

  - Post attributes: `id`, `title`, `body`, `user_id`, `created_at`, `updated_at`
  - Tag attributes: `id`, `title`, `created_at`, `updated_at`
  - Categorization attributes: `id`, `post_id`, `tag_id`

For example, in our Blog app we might want to show a list of tags that have been applied to more than 5 different posts - and clicking one of those tags would take the user to a list of all the posts categorized with that tag.

If you've taken a database class, you might be able to imagine how those SQL queries might look. If you don't have prior database experience, it's still simple to see the parameters that we're looking for in plain terms: 
  - First, we need to get a list of tags which are associated with more than 5 posts. So we'd look for Tags that are connected to more than 5 Categorization records.
  - Then, we'd need to find Posts based on a tag - so from a tag, get all corresponding Categorizations, then get the Post from each Categorization.

Rails helps a lot when you want to retreive database records based on queries like these. When you define a model that inherits from `ActiveRecord::Base`, it comes with a lot of functionality.

###A few basics

Our app has a model called Post, so I'm going to use `Post` as an example here. Any model that inherits from `ActiveRecord::Base` will have the following methods (and more!) available:

  1. **All**: To retrieve *all* of the `Post` records stored in our app's database, we can call `Post.all` (remember that parenthesis are optional in Ruby, so we could also write `Post.all()`). This method returns an [ActiveRecord::Relation](http://api.rubyonrails.org/classes/ActiveRecord/Relation.html) object, which is similar to an array or a list. It is an ordered list of `Post` objects (or whichever model you're working with), though there's some database magic happening behind the scenes for optimization purposes.
  2. **Find**: To retrieve *one* specific `Post` record, we first need to have the ID of the Post that we're looking for. Let's assume it's 12. Then. we'd call `Post.find(12)`, which returns a `Post` object - namely, the `Post` object with an ID of 12. **Note:** `find` will throw an error if a `Post` record with the requested ID does not exist in the database.
  3. **Where**: `find` is great if you know the ID of the `Post` you're looking for, but what if you're looking for one or more objects that meet a certain criteria? Enter the `where` method. Let's say we wanted all of the `Posts` that are titled "Hello, world!". We'd call `Post.where(title: 'Hello, world!')`, which would return an [ActiveRecord::Relation](http://api.rubyonrails.org/classes/ActiveRecord/Relation.html) object, just like `all` did - except instead of returning all `Post` records, it would only include those that meet the specified criteria. **Note**: `where` will *not* throw an error if no matching records are found. Instead, it will return an empty ActiveRecord Relation. 
  4. **Joins/Includes**:  Time-wise, database queries are expensive - so Rails tries to optimize them by only loading the tables that you ask for at query time. `where` on its own is great if you're concerned only with attributes from one model/table, but what if you're searching based on information from another table? Enter the `joins` and `includes` methods. Let's say we wanted all of the `Posts` that are associated with the `Tag` with an ID of 1. Our `Post` model doesn't have a `tag_id` field that we can directly query on, so first we need to call `joins(:tag)` like so: `Post.joins(:categorization).where(tag_id: 1)`. **Note**: You should use `includes` if you need attributes of both models to be returned immediately in your result, and `joins` if you need only the attributes of the original model you're querying on. [A more in-depth explanation is here](http://tomdallimore.com/blog/includes-vs-joins-in-rails-when-and-where/).

There are some more methods that exist to help you work with models ([read the docs or ping me if you'd like to know more!](http://guides.rubyonrails.org/active_record_querying.html)), but these will get us started.

###Posts by Popular Tags
To accomplish this, we need a few different things:
  1. Before we get started, we need to know which page(s) we want this clickable list of tags to appear on, and figure out which controller manages that page.
  2. That controller needs to get a list of tags, which the corresponding view should display in a list.
  3. When a user clicks on a tag, we need to pass the tag that the user clicked on to a new method in a controller that can handle that tag input accordingly.
  4. That controller method needs to get all of the posts associated with the given tag.
  5. We need a view for the controller to return that will display the posts for the given tag. It would probably be best for that page to also display the name of the tag that was clicked, too.
  6. Our goal is for the original list of `Tags` to only include tags that are associated with more than five posts. If we hard-coded in a list of tags to click on, we would need to come back and update it every time a new post was made, or whenever an old post's tags were edited, which doesn't sound like fun. What if we forgot? It's best to automatically generate this list from the data in our database.

It seems like this is a great job for the sidebar on our site. But which controller handles this? It should appear on every page, and the HTML code for the sidebar lives in `app/views/layouts/application.html.erb`, not in one of the model-specific view folders! Rails apps have an application controller (`app/controllers/application_controller.rb`) that is executed on every request, before the request-specific controller method. When you want to do something site-wide like set up some variables to be used in a shared layout, there's a good chance it should go in `ApplicationController` as a `before_action`, so that it gets executed *before* other controller actions.

In `app/controllers/application_controller.rb`, after line __:
```
before_action :initialize_popular_tags

def initialize_popular_tags
  @popular_tags = Tag.all # don't worry, I know we don't want all tags! We'll come back to this later.
end
```

In `app/views/layouts/application_.html.erb`, replace the entire `<ul></ul>` tag and all its content on lines __-__ with:
```
      <% unless @popular_tags.empty? %>
        <ul class="nav-links">
          <% @popular_tags.each do |tag| %>
            <li><%= link_to tag.title, 'http://www.google.com'  %></li>
          <% end %>
        </ul>
      <% end %>
```

Wait - why did we link to google.com? Well, we haven't written the controller method that should get executed when you click on the tag link, yet - so we don't know the path in our Rails application that it should link to. Linking to a different website (or page) that *does* exist is a good way to test that the links are appearing correctly with the proper tag titles.

Let's start to fix that by creating a new controller method to get all of the posts for a given tag. First, we need to decide which controller this method belongs in. Since the records that we're retrieving from the database and returning to the view are instances of the `Post` model, we're going to make this a method in our `PostsController`. But we need to know which instance of the `Tag` model that we're working with in order to get all its posts, so we do need some information about that `Tag` object.

In order to get that information, we'll require that the URL include the ID of the tag object that we're looking for. To accomplish this, we're going to add a new route to our `config/routes.rb` file that maps from a URL to the controller action we're going to create.

In `config/routes.rb` on line __:
```
  get '/tags/:id/posts', to: 'posts#by_tag', as: 'posts_by_tag'
```

`get` means that this route will only apply for HTTP requests with the `GET` method. `'/tags/:id/posts` is the URI pattern that we'd like this mapping to apply for; `:id` means that we're expecting a parameter, which we're naming `:id` (just like when you name a parameter to a function that you're writing).`'posts#by_tag'` tells Rails to look in the `posts` controller for a method called `by_tag` to execute for requests that match that pattern. `as: 'posts_by_tag'` gives us a helper method for this URL (more on that later).

Now, if we tried to go to `class-demo-hsgroves.c9users.io/tags/1/posts`, we'd get an error, because we haven't created a `by_tag` method in our `PostsController` yet. Let's do that now:

In `app/controllers/posts_controller.rb`, after line __:
```
  # GET /tag/:id/posts - this comment helps you to remember which URL will map to this method
  def by_tag
    @tag = Tag.find(params[:id]) # remember, find will raise an error if the record is not found
    @posts = @tag.posts # get a list of all that tag's posts
  end
```

Now that we've got a controller action, we need a corresponding view.

In a **new file** called `app/views/posts/by_tag.html.erb`:
```
<h1>Posts tagged "<%= @tag.title%>"</h1>

<% if @posts.empty? %>
  <p>No posts found.</p>
<% else %>
  <% @posts.each do |post| %>
    <p>
      <h3><%= post.title %> by <%= post.user.email %></h3>    
      <p><%= post.body %></p>
      <strong>Tags</strong>
      <ul>
        <% post.tags.each do |tag| %>
          <li><%= tag.title %></li>
        <% end %>
      </ul>
    </p>
  <% end %>
<% end %>
```

Let's test this out by viewing it: `class-demo-hsgroves.c9users.io/tags/1/posts`

Sweet. But oh - clicking the links in the sidebar doesn't quite work... it still links to google.com! Now that we have a new, working page to link to, let's go back and fix that.

In `app/views/layouts/application_.html.erb`, on line __, replace `'http://www.google.com'` with `posts_by_tag_path(id: tag.id)` (note that that's a function call, and no longer a string!). The full line should look like this:
```
            <li><%= link_to tag.title, posts_by_tag_path(id: tag.id)  %></li>
```

Remember that when we added our new entry to the `config/routes.rb` file, we specified `as: posts_by_tag`. Since we did that, Rails creates a new helper function called `posts_by_tag_path` that we can pass the named parameters that the route requires.

This is great! Next week, we'll change the list of tags on the side to only show popular tags (those related to more than five posts).
