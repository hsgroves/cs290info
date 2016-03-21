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

###Migration

##Changes to Model files

##Updating controllers & views

##Updating ActiveAdmin
