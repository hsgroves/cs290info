##Making Tags
Let's run a quick generator to make a Tag model. `Tags` are pretty simple - they're just a string that we're later going to associate with some `Posts`.

In your terminal:
```
bundle exec rails generate scaffold Tag title:string
```

We do want to make sure that tag titles are both always present (not blank) and unique. We're going to do that two ways: through a Rails model validation, and through SQL table constraints.

In `app/models/tag.rb`, add the following after line 2:
```
    validates :title, presence: true, uniqueness: true
```

In `db/migrate/YYYYMMDDHHMMSS_create_tags.rb`, replace line 4 with the following:
```
      t.string :title, null: false, default: 'untitled'
      t.index :title, unique: true
```

Now, run `bundle exec rake db: migrate` to add our new `tags` table to the database.

##Many-to-Many Relationships
`Posts` won't be related to `Tags` in the same way that `Posts` are related to `Users`. A `Post` can only be associated with one `User`, but a `Post` could have many `Tags`. Likewise, we might want to know how many `Posts` are associated with a given `Tag` - so a `Tag` can't just be associated with one `Post`. We call this kind of relationship a "Many-to-Many" relationship. In Rails, there are two ways of doing this: with a `has_and_belongs_to_many` or a `has_many, :through`.

`has_and_belongs_to_many` should be used when no extra information is expected to be stored about the relationship. `has_many :through` is more robust - it allows you to work with the entity that joins the two models. If you need to store extra information about the relationship between two models (i.e. anything more than just the fact that they are related), you should use a `has_many :through`. *In most cases, `has_many :through` is a better choice, as it is more extendable.* (The Rails guides on Associations state: "You should use has_many :through if you need validations, callbacks, or extra attributes on the join model.")[http://guides.rubyonrails.org/association_basics.html]

##Connecting Posts and Tags: `has_many :through`

There's a good chance that we might want to do other stuff with tags in the future, like listing the most popular ones, ordering the tags on a post, or restricting the number of tags each post can have. Since those are all future possibilities, it'll be best for us to begin by creating this many-to-many association as a `has_many :through`.

###Migration
We're going to write our own migration to create a table to join our `Post` and `Tag` model.

To generate an empty migration file, run the following:
```
bundle exec rails g migration CreatePostsTags
```

Open the resulting file (`db/migrate/YYYYMMDDHHMMSS_create_posts_tags.rb`), and paste the following:
```
def self.up
  create_table 'categories_products', :id => false do |t|
    t.column :category_id, :integer
    t.column :product_id, :integer
  end
end
def self.down
  drop_table 'posts_tags', :id => false do |t|
    t.column :category_id, :integer
    t.column :product_id, :integer
  end
end
```

##Changes to Model files
Just like when we added `Users` to `Posts`, we need to alter the models to tell them about this new association.

In `app/models/post.rb`:
```
has_many :tags, through: :post_tags
```

In `app/models/tag.rb`:
```
has_many :posts, through: :post_tags
```

##Updating controllers & views
Now we need to build out an interface for adding tags to a post when we create it. Like many existing blog sites, we're going to use a comma-separated approach to adding tags - so we'll have one text input for all of a post's tags, and expect the user to enter them as a comma-separated list.

To be clear, here we're asking the user to input a list of tags, but we have to save these tags as separate entities in our app's database. A `Post` has a collection of `Tag` models that we can access through `@post.tags`, but the user is just inputting a string of text. So we're going to use a fake field in our form to allow the user to input text, rather than deal directly with models in the system.

In `app/views/post/form.>???????`, add the following after line __:
```
<% f.label :tag_titles %>
<% f.text_field :tag_titles %>
<%= errors_for(@post, :tag_titles) %>
```
Now when we go to create a new post, we see an input for our tags. But when we submit the form, no information about the tags is saved - let's add that logic to our posts controller (`app/controllers/posts_controller.rb`) now.

First, let's create a method to split up the string into an array of individual tags. Under the `private` keyword on line __, add the following:
```
def get_tags(str, delim)
  tag_arr = str.split(delim)
  tags = []
  tag_arr.map do |title|
    next unless title.strip!.length # we don't want to add blank tags
    tags << Tag.find(title: title) || Tag.create(title: title)
  end
  return tags
end
```

Next, we need to tell the `PostsController` to accept our parameter called `tag_titles`. In the `permit_params` function, add
`:tag_titles` to the list of parameters passed to `.allow(`.

There are three methods that will need to deal with tags - `update`, `edit` and `create`. `create` is more straightforward, so we'll start with that.

After line __, add the following:
```
  tags = get_tags(params[:tag_titles], ',')
  tags.each do |tag|
    post.tags << tag
  end
```

Now we need to make sure we populate the tags input on the `edit` form for an existing post. Within the `edit` method, on line __, add the following:
```
  @post.tags = 
```

`update` is a little more complicated - if the `post` already has a certain `tag` associated with it, we don't want to add it again; on the other hand, if a tag is no longer in the list, we want to make sure we remove it. To keep things simple, we're going to clear out the `post`'s tags, and re-add the ones from the user's input. So after line __, add the following:
```
  tags = get_tags(params[:tag_titles], ',')
  @post.tags.destroy
  tags.each do |tag|
    post.tags << tag
  end
```

###Showing a Post's tags
At this point, we can add and edit the tags associated with a post - but when you view a post, you can't see its tags! Let's fix that.

In `app/views/posts/show.html.erb`, add the following after line __:
```
<% if @post.tags %>
  <ul>
  <% post.tags.each do |tag| %>
    <li><%= tag.title %></li>
  <% end %>
  </ul>
<% end %>
```

##Updating ActiveAdmin

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
