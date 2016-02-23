[Ruby points of interest!](https://github.com/hsgroves/cs290info/blob/master/ruby_topics.md) We'll go over these as a class next week.

###Schemas
A **schema** is the structure skeleton of a database. It defines the database's tables, their columns, as well as information, relationships, rules, and constraints on those columns and tables.

###Schema migration vs data migration
A **schema migration** changes the structure or constraints of your database. Ex: Our application had an empty database before we created and ran a schema migration to create a table to contain our blog posts.

A **data migration** updates data in the records that are already stored within the database. Ex: Imagine our `posts` table had a `post_type` field and that when we added the field, we decided to let users input a custom `post_type`. Now, we want to limit all posts to one of the following: `JOURNAL_ENTRY`, `DISCUSSION`, `REVIEW`. We have a bunch of user-generated types already in the system, so we'd need to write a **data migration script** to update each old type to a valid new one.

It's usually good practice to keep schema migrations and data migrations in separate files. In Rails, it tends to be easy to "roll back" a migration if it doesn't work or produce the desired effect; keeping schema and data migrations separate ensures that updating values and changing columns or tables all happens in the correct, intended order.

###HTTP & its methods
HTTP (Hypertext Transfer Protocol) is the protocol (or set of rules) that determines how information is transferred via hypertext (ex: on the internet).

####HTTP Methods and available scopes:

* GET	- collection, resource: retrieve an object or set of objects
* HEAD - collection, resource: retrieve headers from an object or set of objects
* POST	- collection:	create a new resource in a collection
* PUT - resource:	update a resource (with an entirely new set of values, or without affecting other resources)
* PATCH - resource:	update a resource (with only some new values, or when this update affects other resources)
* DELETE -	resource:	delete a resource
* OPTIONS - 	return available HTTP methods and other options

[largely paraphrased from here](http://restful-api-design.readthedocs.org/en/latest/methods.html)

Note that in practice, it's not always commong to differentiate between `PUT` and `PATCH`. `PUT` is more frequently used and usually perfectly acceptable.

A few of these methods map to the basic CRUD (create, read, update, delete) actions that we talked about last week:

* `POST` => create
* `GET` => read
* `PUT` => update
* `DELETE` => delete

####Actions in rails
`bundle exec rake routes` output

###HTTP Responses
Typically, you see an HTTP response in your browser as an HTML page. Frequently, web servers respond with raw data instead of a full HTML page.

These responses are used by the application that made the HTTP request to update data or objects and display information within the application.

Any application (desktop, embedded, mobile, web...) that is capable of connecting to the internet can make an HTTP request. Web browsers are designed to display HTML pages; other applications may not be. JSON and XML allow these applications to get data in a way that's more easy to process than HTML, which is an irregular tree structure.

Front end web applications are frequently very complex Javascript applications that run entirely within a web browser. In this case, the initial HTTP request returns an HTML page that contains Javascript code. The JS executes within the page, and attaches handlers to various events (like clicking a button or submitting a form) that spawn more HTTP requests. Instead of returning a full HTML page, the server returns a message or JSON object, which the Javascript application recieves and handles appropriately.

###Devise and ActiveAdmin
[Devise]() is a free, open source Ruby gem for Rails authentication.

[ActiveAdmin](https://github.com/activeadmin/activeadmin) is a free, open-source Ruby gem that contains a Rails engine (a mini Rails app that runs within another Rails app). It gives you a user system and *extremely* robust administrative interface with minimal setup.
