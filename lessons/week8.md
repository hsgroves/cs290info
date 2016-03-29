##HTML, CSS, & Javascript

* HTML defines what content appears on a page
* CSS defines how that content appears visually
* Javascript is the only scripting language supported by modern web browsers to handle on-page interactions and programmatic functionality

###A few notes regarding Web Design
You're probably already aware that "web design" and "web development" are vastly different things. If you're not visually or artistically inclined, that's okay! Chances are, when you work for a companyy, they'll pay a dedicated visual designer to design how your pages ought to look. For your own personal projects though, you probably don't want to pay someone. For that reason, it's usually helpful to know a thing or two about visual design, or at the least about some current trends.

This is a programming course, and if you'd rather focus on a functional app than one that looks pretty, that's fine. On the other hand, if you'd rather make a basic app and spend time making it look amazing, I'm perfectly happy with that as well.

Either way, knowing about HTML and CSS will do nothing but make your life easier - especially if you end up doing any kind of web application development in the future.

##HTML Basics & Document Structure
HTML is not a programming language - it's a markup language (if you're curious or don't know, HTML stands for *H*yper*T*ext *M*arkup *L*anguage). That means that there's no functionality or control statements built into HTML itself - it's just a way to tag sections of text in different ways.

HTML pages are not regularly structured. An HTML document is translated by the browser into a tree of nodes - if you've played with Javacript and heard about the `DOM` or "Document Object Model", that's what it's referring to.

So at their core, HTML pages are text grouped into different categories. Grouping is done by "tags". An HTML tag is wrapped in angle brackets (`<>`). Groups of text wrapped in tags are called elements, and there are many different types of elements.

Most types of elements require both an opening tag to denote where the element starts, and a closing tag to denote where it ends. Anything between the opening and closing tags is the content of the element. HTML elements can contain other elements; this is how the tree structure of an HTML document is formed.

If element A contains another element B, we say that A is B's `parent`, and B is A's `child` - if you're familiar with tree data structures, this is not new.

Your elements should be *nested* properly. By "nested", I mean that if an opening tag for element A comes before the opening tag for element B, then the closing tag for B should come before the closing tag for element A, so that B is entirely inside of A. If you're still thinking of this like a tree, this follows from the fact that each child element can have only one parent.

The basics:
```
<!DOCTYPE html> <!-- A DOCTYPE declaration is good practice. It tells whatever is parsing this file (usually a web browser) that it contains HTML.  -->

<html> <!-- The html tag is the root of the document tree. Nothing (except a DOCTYPE declaration) should come before or after it. -->
<!-- This is a comment. Note the opening and closing tags. Comments are ignored by the browser,
just like compilers and interpreters ignore comments in programming languages.
They can span multiple lines. HTML largely ignores/collapses whitespace. -->

<head> <!-- Things within the head tags do not show up on the page itself.
  Things like page title, stylesheet links, javascript sources, and metadata for search engines go here. -->

  <!-- It's good practice to indent children of an element, typically with two or four spaces.
  It also helps to make sure all of your tags are nested properly. -->

  <title>This text will show up in the browser's tab or top bar menu.</title>

  <link rel="stylesheet" type="text/css" href="style.css">
  <!-- This is an example of a tag that does not require a closing tag. 
  It links to an external CSS file (by the content of the href attribute) and
  tells the document to apply those styles to the elements in this document. -->
</head>

<body> <!-- Stuff within the body tags DO show up on your page.
  This is where your content goes! -->
  
  <p> <!-- p stands for paragraph. -->
    Get your shit together, Summer.
    <!-- "Get your shit together, Summer." will actually show up on the page. -->
  </p>
  
</body>

</html>
```

Of course, you can add more information to an element than just the content between the tags. Something like a link tag (`<a>`, which stands for anchor) needs to know more - like where the link should take you. So we add an attribute (`href`, or *h*yperlink *ref*erence) to tell it:
```
<a href="http://google.com">This will take you to Google.com!</a>
```

Like there are many elements, there are many different types of attributes. Javascript, and especially modern Javascript libraries (jQuery, AngularJs, React, etc) offer support for adding your own arbitrary attributes and defining their behavior, but plenty have built-in support in browsers with no extra Javascript necessary.

##Web Development Without WiFi
Browsers can handle HTML, CSS, and JS - that's their purpose. So if you have HTML, CSS and JS files locally, you can view them in a web browser without having an internet connection. The way to do so varies slightly by OS, but the general idea is to either a) from Chrome, go to File -> Open and find the HTML file on your computer, or b) from your computer's filesystem, choose to open the file with Chrome.

Note: You do need the files to be saved with the proper extension (.html, .css, .js) 

##CSS
CSS stands for *C*ascading *S*tyle*S*heets. Stylesheets are are ordered lists of style rules to apply to elements.

A style rule looks like this:
```
// single-line comment
p {
  color: #000;
}
```
`p` is the HTML element we want to target. In CSS, we call it the "selector", because it's possible that more than one element is contained (see below). 

`color` is the property of the selected elements that we want to alter. Properties are predefined by the browser that you're viewing a page in. Some browsers support properties in different ways; some browsers don't support certain properties at all. This is why web developers hate IE, especially old versions. This is why "browser compatibility" is a thing.

`#000` is the value that we're giving the `color` property. For something like color, the value is a hex code representing an RGB value. Different kinds of properties will have different kinds of values.

##Selectors and inheritance
Consider the following CSS:
```
p {
  color: #FFF; // white
}

p {
  color: #000;
  background-color: #DAF98C; // a nice lime green
}

p span {
  color: #C50D23; // red
}

p, span {
  font-weight: bold;
}
```

That will produce the following effects:
```
<p>
  Black, bold text on a lime green background
</p>

<p>
  <span>Red, bold text on a lime green background</span>
</p>

<span>Black, bold text on a white background</span>
```

Note how later, different rules like `font-weight` for the `p` element do not cancel out the `color` and `background-color` property, but setting a property that has already set (like setting `color` a second time) will override the first value.

Also notice how a space between `p` and `span` denotes that following style rules should only apply to `span` elements that are contained within a `p`; a comma between `p` and `span` denotes that following style rules should apply to *both* `p` and `span` elements.

Those last two are far from the only methods of selecting elements to target with your CSS rules; we call them 'CSS Selectors'. Going through all of the CSS selectors and how to use them would take up a long time. If you're interested in CSS, you should look through documentation on selectors on your own time. It gets pretty crazy (or pretty fun, depending on how you look at it!).

###Helpful Links

  * [MDN CSS Selectors Reference](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started/Selectors)
  * [MDN CSS Properties List](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference)
  * [ColourLovers for premade color palettes](http://www.colourlovers.com/palettes)

###divs and spans, block and inline
`<div>` and `<span>` are two important HTML elements. The main difference between them is that divs are **block-level** and spans are **inline** elements. Block-level elements by default take up their own line on the page, and you can assign them a width and height. Inline elements do not break the text flow of the HTML page, and adjust their width and height to their content. There's also something called `inline-block`, that combines the best of both worlds.

This is a very good first CSS thing for you to understand, and I highly recommend reading the following two pages on your own time. [StackOverFlow explains it visually.](http://stackoverflow.com/questions/8969381/what-is-the-difference-between-display-inline-and-display-inline-block) [This article explains it technically.](http://www.impressivewebs.com/difference-block-inline-css/)

Anyways, a `div` will take up its own line on the page. I like to think of divs as invisible boxes that are used to organize the layout of a page. They're used to wrap large portions of text/elements, and great at controlling the flow of a document.

A `span` is usually used to wrap a small section of text. I like to think of spans as a highlighter to style different types of texts (labels, etc).

##Chrome Developer Tools Demo - view ALL the source code
Check out [this ecommerce page](http://www.maydesigns.com/shop/photobooks/build/all-layouts/#/).

Several options:
1) View -> Developer -> View Source (not as good)
2) View -> Developer -> Developer Tools (*sweet)
3) Right click on any element on the page -> Inspect Element (*sweet* like above, but targets that specific element)

##Demo
The best way to learn HTML and CSS is by doing it. Even trying to recreate a site that exists already can be a great learning exercise.

I'm copying and pasting a bunch of raw text data from the [Mr. Robot season finale discussion thread (spoilers, don't click if you haven't seen it)](https://www.reddit.com/r/MrRobot/comments/3jfpuu/mr_robot_s1e10_eps110_zer0dayavi_official/) on Reddit (a prime example of a very popular site with absolutely awful design), and together we're going to go through changing raw text into HTML and CSS.

No spoilers below.

```
[–]XYEaQMZJvS 661 points 6 months ago 
"You should just get a BlueTooth. That way everyone will think you're just the local douche."
permalinksavegive goldpocketbuffer
[–]tRon_washington 43 points 6 months ago 
I might just get one so I can start yelling at people in public
permalinksaveparentgive goldpocketbuffer
load more comments (1 reply)
buffer
load more comments (2 replies)
buffer
[–]rflairfan1Mr. Robot 440 points 6 months ago 
If I call 911 will that summon Christian Slater?!?!?
permalinksavegive goldpocketbuffer
[–][deleted] 185 points 6 months ago 
I just tried it! Totally works!
permalinksaveparentpocketbuffer
[–]rflairfan1Mr. Robot 37 points 6 months ago 
That must be why it didn't work for me. Damn you!!!!
permalinksaveparentgive goldpocketbuffer
load more comments (1 reply)
buffer
load more comments (5 replies)
buffer
[–]zcrates66Angela 183 points 6 months ago 
"I don't know who you think you are talking to, but I'll try the Pradas next" hahah
```

Cool. Let's make it look like it does on Reddit (see comment).

Finished code will be pasted here after class.

reddit.html:
```
```

reddit.css:
```
```
