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
  2. When a user clicks on a tag, we need to pass the tag that the user clicked on to a new method in a controller that can handle that tag input accordingly.
  3. That controller method needs to get all of the posts associated with the given tag.
  4. We need a view for the controller to return that will display the posts for the given tag. It would probably be best for that page to also display the name of the tag that was clicked, too.
  5. Our original list of `Tags` should only include tags that are associated with more than five posts. If we hard-coded in a list of tags to click on, we would need to come back and update it every time a new post was made, or whenever an old post's tags were edited, which doesn't sound like fun. What if we forgot? It's best to automatically generate this list from the data in our database.

It seems like this is a great job for the sidebar on our site. But which controller handles this? It should appear on every page, and the HTML code for the sidebar lives in `app/views/layouts/application.html.erb`, not in one of the model-specific view folders! Rails apps have an application controller (`app/controllers/application_controller.rb`) that is executed on every request, before the request-specific controller method. When you want to do something site-wide like set up some variables to be used in a shared layout, there's a good chance it should go in `ApplicationController` as a `before_action`, so that it gets executed *before* other controller actions.

In `app/controllers/application_controller.rb`, after line __:
```
before_action :initialize_popular_tags

def initialize_popular_tags
  @popular_tags = Tag.all # don't worry, I know we don't want all tags! We'll come back to this later.
end
```

In `app/views/layouts/application_.html.erb`, after line __:

 # Tag.joins(:taggings).select('tags.*, count(tag_id) as "tag_count"').group(:tag_id).order(' tag_count desc')
