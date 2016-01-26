# Week 1
This week is all about getting started & setting expectations for the course.

## My Expectations
- This is a one-credit, entirely project-based course
- I'll put in as much effort as you do - if you're learning and excited, I'll teach you whatever you want to learn; if you're not, you won't learn anything useful, but will have a super easy semester. Either is fine with me!
- Participation and other typical factors are *not* a factor in your final grade. However, you **will** fail if you do not attend, since all of the materials & knowledge you need for the project will be explained during class time. Show up and pay attention :)

## Slack
[Slack](http://www.slack.com) is a chat system used by lots of companies as an internal communication tool. It's great. All of our class communications should occur on the class slack channel, [http://cs290spring16.slack.com](http://cs290spring16.slack.com). Slack is available as a web-based app as well as a native OSX, Windows, Linux, iOS, Android, and Windows Phone app.

You'll need an invite to join; shoot me an email at hgroves@adelphi.edu from the email address you'd like to receive an invite from and I'll send you an invite.

Once you're invited, you can choose a username and join. A Slack team consists of different channels (chat rooms). I've created a few already for several different topics; you're welcome to create more at any time for any reason. You can private message me or any other student through Slack, as well.

## How to get the most out of this course
I'm here to teach, guide, and answer questions. However, nothing I tell you will stick without you trying it and doing things on your own - so if there's an in-class demo, try it on your own after class. If you want to do more than what I explicitly show in class, you should! 

The most important thing that I would like all of you to learn through this course is not a specific technology, language, or framework, but *how to figure things out on your own*. When you graduate and get a job, your first day *will* mostly consist of you realizing how much you *didn't* learn in college. That's scary - unless you're already comfortable with stuff like looking up "Function.prototype.call vs Function.prototype.apply", searching through StackOverflow to find answers pertaining to the specific version of MySQL your company uses, and **actually reading the docs**.

For this reason, if you ask me a question about how to do something, I *expect* that you've at least Googled it first, haven't found anything, and are resorting to asking me after the fact. This is not to dissuade you from asking me questions, but instead to encourage you to learn *how to learn* on your own. In four months, this course will be over; that shouldn't be the end of your learning.

## What you can do to get a jump start
- Codecademy courses (good for learning syntax and gaining basic familiarity):
  - [Ruby](https://www.codecademy.com/learn/ruby)
  - [HTML/CSS](https://www.codecademy.com/learn/web)
  - I *do not* recommend paying for extra materials on Codecademy. The free materials are more than enough for you to to get familiar with the basics.
  - I *do not* recommend taking the Rails course on Codecademy. Codecademy caters towards people who are not programmers. You are a programmer. You don't need the hand-holding it provides.
- Books (There are no textbooks for this class. If you're a fan of books and learn well from them, these are my recommendations. Look around for good prices :wink:)
  - [HTML & CSS: Design and Build Websites](http://www.htmlandcssbook.com/)
  - [HTML and CSS: Visual Quickstart Guide](http://www.peachpit.com/store/html-and-css-visual-quickstart-guide-9780321928832)
  - [Ruby Pickaxe Book](http://www.amazon.com/Programming-Ruby-1-9-2-0-Programmers/dp/1937785491) - this is a dense book and now slightly outdated, but very highly regarded in the Ruby community
  - [The Rails 4 Way](http://www.amazon.com/Rails-Edition-Addison-Wesley-Professional-Ruby/dp/0321944275/)

## Web Application Development Overview
- web site vs. a web application
- web app development process
  - conception ($$)
  - prototype
  - feedback
  - updates
  - release

Cool, but how do you actually do that? Programs aren't run from the command line or in BlueJ in real life...

### Server Intro
- development environments:
  - local
  - staging
  - production
  - etc
- "deploying": putting updated code into an environment
- configuring web servers: will get to it if we have time

### Typical software stack
How stuff interacts in a web app:

1. OS of the server itself (Linux, usually)
2. Database (common flavors: MySQL, PostgreSQL, also noSQL databases - MongoDB, Redis, Couchbase)
3. ORM (Object-Relational Mapping)
4. Back-end language (Ruby in our course; could be Python, Java, PHP, Javascript, or pretty much anything else)
5. Web server (Apache, nginx, etc)
6. HTML, CSS, and Javascript
  * JS frameworks (AngularJS, React, Backbone, Ember... new ones are popping up all the time)

Common stacks (you know it's popular when it's got a fancy acronym):
- LAMP stack (Linux/Apache/MySQL/PHP)
- MEAN stack (MongoDB, Express, AngularJS, Node.js)
- Java web application stack (Linux or Solaris/Tomcat/MySQL/JSP)

### Where Rails fits in
Rails is a library (or `gem`) in Ruby that provides a bunch of stuff related to levels 3, 4, 5, and 6 in the above ^ list. Among other things, it: 
- Provides an ORM, [ActiveRecord](http://guides.rubyonrails.org/active_record_basics.html), that allows you to interact with a database without writing raw queries.
- Provides a [convention-based](https://en.wikipedia.org/wiki/Convention_over_configuration) framework for writing models (the objects that exist in your application), controllers (the function that is executed for a particular HTTP request's URL), and views (the HTML that gets sent to the browser after a controller function returns)

I chose Rails for this course because it minimizes the amount of low-level decisions that you need to make as a developer, has an abundance of documentation + support available for free online, and will give a fairly full overview of the entire web application development process.

## Ruby/Rails Install Options
Use your own computer, not an AU computer (unless you choose to develop using Cloud9). **Do not** use pre-built Rails installers. It's very likely that they won't work or won't set things up properly/with the right versions.

### Windows
If you're using a Windows machine or an older Mac (pre-2012), I highly recommend using [Cloud9](https://c9.io/pricing/webide) to develop remotely (the free option is fine). You will need an internet connection at all times in order to develop.

Alternately, you can configure [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [Docker](https://www.docker.com) and install Ruby/Rails on a Linux-based virtual machine. VirtualBox is a bit simpler to set up than Docker; both can be very difficult to configure. To start, Cloud9 is probably a better choice.

### Mac
I very highly suggest [this guide](http://railsapps.github.io/installrubyonrails-mac.html) to help you set up Homebrew, Ruby, RVM, Git, and Rails.Note that version numbers may be slightly out of date (though shouldn't be by much).

If you run into strange errors in the process and are unable to fix them, consider [Cloud9](https://c9.io/pricing/webide) or a [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [Docker](https://www.docker.com) installation.

### Linux
For Ubuntu Linux, I recommend [this guide](http://railsapps.github.io/installrubyonrails-ubuntu.html) (through the "Create a Workspace Folder" heading). Note that version numbers may be slightly out of date (though shouldn't be by much).

## A note about web browsers
Browser compatibility is less of an issue than it was even two years ago, but still makes things a headache (and usually at the worst times). So that everyone is on the same page (literally), I ask that you use [Google Chrome](https://www.google.com/chrome/) for everything in this course, even if it's not your preferred browser. 

Start getting comfortable with the [Chrome Developer Tools](https://developer.chrome.com/devtools), too.

## A note about text editors
I don't care what you use to write your code, as long as it's an editor specifically geared towards writing code(*not* Notepad, TextEdit, MS Word, etc). I like [Sublime Text 3](http://www.sublimetext.com/3), which is free (though they ask you to buy, there's no time limit on the free trial). If you're into Vim, that's fine. If you find a full-featured Ruby IDE that you like, that works too.

## A note about Git and Github
Version control is a huge part of professional coding. Git is not the only version control system out there, but Github is fairly widely used, and it'll do you good to get familiar with using it. Most OSS (open-source software) is housed on Github.
  
# For next week
1. Shoot me an email at hgroves@adelphi.edu from the email address you'd like to receive an Slack invite from, create your Slack account, and post your favorite gif in `#random`.
2. Start setting up your Ruby & Rails environment. If you run into problems, ask Google, a classmate, or me (via Slack).
3. Download Chrome, if you don't already use it. If you do, make sure it's up-to-date.
4. Pick an editor or IDE.
5. Create a Github account, if you don't have one already.
6. If you feel like it, go through the Codecademy Ruby and HTML & CSS courses.
7. Let me know at any point if you have questions/comments/concerns!
