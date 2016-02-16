#Week 3 - Starting our Rails App

* Note: this and all upcoming lessons will be demo'd using Cloud 9 with a Ruby workspace.

##Rails App Directory Structure

##Creating our first model
Objects in a Rails app are called a `Model`. For our blog app, we're starting with the most important model first - a `Post`.

###Post Attributes
1. Some kind of unique identifier
2. Post date/time
3. Contents (text) of the post
4. Title
5. User who made the post
6. Tags (*)
7. Comments (*)

Read: [ActiveRecord Naming Conventions](http://guides.rubyonrails.org/active_record_basics.html#convention-over-configuration-in-active-record). **Reminder:** ActiveRecord is the ORM (object-relational mapping) that is used to translate SQL tables into Rails model instances and vice-versa.

`bundle exec rails generate scaffold post .....` or `bundle exec rails g scaffold post .....`

This command generates and modifies a bunch of files automatically, setting up basic CRUD (Create, Read, Update, Delete) actions and a user interface that will allow you to use the model you just generated.

**Note:** This is the first and last time we will generate a scaffold to create a new model in this rails project - look at all the overhead! Extra files, etc... :no_good: In the future, we'll manually add only the files that we need. For your project, you should use whatever methods you're comfortable with!

##Database Information and Migrations

`bundle exec rails dbconsole` or `bundle exec rails db` - this command opens a SQLite console that allows you to interact with your database directly (with SQL statements). Note that you should *never* make schema updates to your database through the console - writing schema migrations through Rails makes your changes easy to write, trackable, and reproducible.

For your (and my!) reference: [SQLite Commands](http://www.tutorialspoint.com/sqlite/sqlite_commands.htm)

To view columns in a table through the SQLite CLI:
`PRAGMA table_info('TABLE_NAME_HERE');`

To migrate your app's database:
`bundle exec rake db:migrate`

##Routes & Views
To see the routes (`URL -> Ruby function` mappings) for your app, run: `bundle exec rake routes`.

##Tracking your project on Github
Have your Github username and password handy.

In order to view your files & commits on Github, you have to configure git to store your files on Github's server. [Github's own article](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/) on how to do so is by far the best explanation of how to do this, and what we're following in class.
