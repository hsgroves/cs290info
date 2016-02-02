#Week 2

###Ruby/Rails setup options:

- **All Platforms:**
[Cloud9 Setup](http://c9.io) - create a public, hosted workspace and choose the `Ruby` template -- *not* `Ruby on Rails`, which has slightly out-of-date versions of Ruby and Rails.
- **Windows:** [Dual Boot Linux](http://www.pcworld.com/article/2955460/operating-systems/dual-booting-linux-with-windows-what-you-need-to-know.html) || [(another article)](http://www.howtogeek.com/214571/how-to-dual-boot-linux-on-your-pc/)
- **Mac:** [Install Homewbrew, RVM, Ruby, Rails, Git, etc](http://railsapps.github.io/installrubyonrails-mac.html)
- **Linux:** [Install RVM, Ruby, Rails, etc (this guide is specifically for Ubuntu, but other Linux distros are likely very similar)](http://railsapps.github.io/installrubyonrails-ubuntu.html)

###[Final Project Requirements](https://github.com/hsgroves/cs290info/blob/master/project_requirements.md)

###Git Overview

###

###Starting a first Rails App (non-Cloud9)

From your terminal, navigate to the folder you'd like to put your application in. Run:

```rails new APP_NAME```

This will create all of the files and folders necessary for a Rails app.
**NOTE:** if you have MySQL or another database management system installed and you'd like to use it, you can specify that by adding `--database=mysql` to the end of the `rails new` command.

To run your app:

```bundle exec rails server``` or ```bundle exec rails s```

**Why `bundle exec`?** There will probably come a time when you have multiple Ruby scripts or Rails applications in existence on your computer. Over time, different applications may require the same gems, and potentially different versions of them. Using `bundle exec` within a project looks for a `Gemfile` that lists the required version numbers for this current project and ensures that the proper versions of all required gems are installed and used. It also ensures that if multiple people are working on the same project, they all are using the exact same gems [(all paraphrased from the Bundler docs)](http://bundler.io/rationale.html).

You'll be typing `bundle exec` a lot; you may want to create an alias (like `be` or `bx`) for it in your bash profile (directions for this vary slightly depending on your OS; if you get stuck, shoot me a message!).

You'll see something along the lines of the following:
```
[2016-02-02 15:54:48] INFO  WEBrick 1.3.1
[2016-02-02 15:54:48] INFO  ruby 2.1.4 (2014-10-27) [x86_64-darwin13.0]
[2016-02-02 15:54:48] INFO  WEBrick::HTTPServer#start: pid=56948 port=3000
```

Congratulations! You can now go to http://localhost:3000 to view your first web application.
