---
title: Flesh on the bones
date: 2023-02-23T09:12:00Z
---

9:12am

The next bit of the process is still about the basics, but it's about putting more flesh on the bones of the website.

Somehow it's important to me that I can imagine this is all getting set up in a way that will allow more growth in the future.
I imagine eventually I'll be wanting to do and learn things about computer life that will go way beyond a static website -- like learning new languages, or going through the Teach Yourself CS series, or building a habit tracker app, or building the amino acid app for Irene.
But I want a place where I can document the process around all of these things.

I want to document so that I can come back and refer to stuff that I forget.
And I want to document because I like to document.
It feels good to me to spend time in this way.
It's alone time.
It's my time.
I'm not even sure it's productive, but it's for myself.

I'd like to have a page of the website that's about stuff I want to work on... ideas I have for the future.
All those app and project ideas can go in there.

Anyway, the point is that I'm starting with a website, a collection of stuff that I'm working on, that I can organize and display using hugo to build HTML files.
And it's a catchall, like a journal or a notebook or a sketchbook.
And if I set it up in the right way, then it will provide an environment for me to do and document this other work I want to do.

SO, back to the walkthrough.

There's a section here where the author of the tutorial shows how to set up CSS.
He is going to set up a Sass stylesheet.
Sass is actually its own language, apparently.
It's billed as "the most mature, stable, and powerful professional grade CSS extension language in the world."
"CSS with Superpowers"

The /static/ directory is where all static assets live, and if there were a plain CSS file, it would go in the static directory.
BUT, Sass needs to be processed into a CSS file.
So it's not a static assett.
For assets that need some form of processing, there is the /assets/ directory.
So, inside /assets/ I'll make a directory called sass.

And I'll paste in the Sass file the guy provides:

body {
  width: 400px;
  margin: 0 auto;
  font-family: sans-serif;
}

nav ul {
  list-style: none;
  padding: 3px 5px;
  background: #111;

  a {
    color: #fff;
    text-decoration: none;

    &:hover {
      text-decoration: underline;
    }
  }

  li {
    display: inline;
  }
}

footer {
  background: #f2f2f2;
  padding: 2px 2px;
  font-size: .7em;
  text-align: center;
}

#map {
  height: 300px;
}

If I run the development server, is this Sass file going to change anything?
Nope, because none of the pages *use* this sass file yet, because this sass file isn't referred to by any of the templates.

So I have to add it to the templates.
Easiest way to do this is to add it to baseof.html, because that template is the base of the other templates... so I add it in one place and it gets added to all the places.

The code to add is in a format I haven't seen yet, so I'm going to take it slow.
{{ $style := resources.Get "sass/main.scss" | resources.ToCSS | resources.Minify }}
<link rel="stylesheet" href="{{ $style.Permalink }}">

I've seen the double curly braces before, when I looked at layouts.
In the layouts, the double curly braces are places that render content from other files.
So in this case, we're rendering content from somewhere outside the baseof.html file

I'm not sure what it means when there is the "$style" word, but I think it's sort of declaring and assigning a variable $style.
the operator ":=" is maybe the start of a process?
I see a bunch of method calls in a row...
There must be an object called "resources" because there are method calls
1. resources.Get "sass/main.scss" (get that file)
2. resources.ToCss (transform it into CSS)
3. resources.Minify (minify? what exactly does that do? does it produce the Permalink that is used in the next line of code?)

And then the <link> tag refers to "{{ $style.Permalink }}"
Seems like I understand enough to keep moving.

There are these questions in my head about what I think is variable declaration and assignment using the := operator, and then I think this is also an example of piping, where the variable gets built through a process of method invocations.
I think maybe these conventions come from the Go language.

One last thing I can tell from looking at the CSS is that there will be things later on in this tutorial that will pull from this sass file.
There's going to be an unordered list for the navigation.
There's going to be a footer with a certain background color and text aligned in the middle.

And there's something called #map - I'm not sure what that does.

But when I run the development server now, the only thing that happens comes from the body tag.
The text moves over and the font changes.


The next part of the tutorial is about partials.
"The idea behind a partial is simple: it's a file that can be included into a layout to reduce repetition or simply hide some complexity.
You'll add a nav bar to your site with a partial.
While you could add this logic directly to your baseof.html, sometimes it's nice to split a layout up into smaller partials so you don't need to deal with a 2000-line file."

So, the nav partial:
create a /partials directory in /layouts/
add the following contents:
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about/">About</a></li>
  </ul>
</nav>

Then include the partial in the baseof.html layout... with the line:
{{ partial "nav.html" }}
(again, the curly braces denote a piece of code that will render content from another file)
(sidenote, when I think about this, I think about how hugo is a program that is going through all these files and at various points INJECTING content from one place into these placeholder {{ ... }}
This is happening at runtime.
So this line goes into baseof.html, which now looks like this:

<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>{{ .Page.Title }}</title>
  {{ $style := resources.Get "sass/main.scss" | resources.ToCSS | resources.Minify }}
  <link rel="stylesheet" href="{{ $style.Permalink }}">
</head>

<body>
  {{ partial "nav.html" }}
  {{ block "main" . }}
  {{ end }}
</body>
</html>

So I can see that now there is a {{ partial ... }} and a {{ block ... }}, and those are two different things.
And when the pages render, now there is a nav bar.
And the CSS comes into play, the nav bar is black and the text is white and the text underlines when I hover...

SO, the partial is a building block.
Other words could be "element" or "building block" or I think in react, these were like separate "Functions"?
I could make a footer partial as well.
I could make a sidebar partial.
And if these are included in the single.html layout or whatever layout, then every page on the website that refers to those layouts will have those partials included.

The tutorial wants to refactor baseof.html a bit to demonstrate more about the power of partials.
It wants to make a <head> partial.

What's interesting is that when I do the first part of the refactor and create the meta.html partial as so: 

<meta charset="utf-8">
<title>{{ print .Page.Title }}</title>
{{ $style := resources.Get "sass/main.scss" | resources.ToCSS | resources.Minify }}
<link rel="stylesheet" href="{{ $style.Permalink }}">

The thing that has changed is that instead of just having the {{ .Page.Title }}, there is a new function call being made to "print" the page title.
And that means the variable will need the context of the current page.
Without changing *something* to provide the context of the current page, the code won't work.
It *will* build, but the title will not show up correctly in the tab in the browser.
Instead of "Home" or "About" the browser will show <nil>

What's cool is that I noticed this problem in the broswer, but I can also see there is a problem through error messages given to me by the development server.
ERROR 2023/02/23 10:04:43 render of "home" failed: "/Users/dreamer/lowerpower/layouts/_default/baseof.html:4:5": execute of template failed: template: _default/list.html:4:5: executing "_default/list.html" at <partial "meta.html">: error calling partial: "/Users/dreamer/lowerpower/layouts/partials/meta.html:2:22": execute of template failed: template: partials/meta.html:2:22: executing "partials/meta.html" at <.Page.Title>: invalid value; expected string

The reason is that I forgot one little detail.
I have to go back to the baseof.html file and add in a little "."
So now baseof.html looks like this:

<!doctype html>
<html>
<head>
  {{ partial "meta.html" . }} <---- This little period is important!!
</head>

<body>
  {{ partial "nav.html" }}
  {{ block "main" . }}
  {{ end }}
</body>
</html>

"That little "." at the end is passing the context of the current page, which allows the partial to print out the current page's title.
You'll see this come up a lot in your Hugo sites, just you wait."

The next page is about templating.
It's good! And I'm tired from what I've just covered here, so more in the next entry.

2023-02-23 10:24am

2023-02-23 10:39am
Templating
The double curlys "{{" and "}}" is templating.
This is inherited from Go templating.
Templating allows you to render stuff from another location in the file system.
Templating also allows you to use variables, loop over arrays, check conditions, and run functions.
Maybe more?
Anyway, when Hugo sees the curly braces, it is ready to DO STUFF inside those curly braces.

"Hugo uses Go templating as its templating language in layouts."
One way to use it is for Output.
That's the rendering ocntent from other places that I've used so far.
You can render page variables from front matter.
You can render global variables from the config.toml file.
This is taking information from one 'central' location and making it available to be rendered or injected through a page.

You can also use templating for inputing a variable.
Variables have the syntax $... and are assigned with := and strings require double quotes ""
{{ $favorite_food := "stew" }}

You can also use templating to check conditions and use certain outputs only if certain conditions are met.
You can also iterate over an array or slice using range.

<!-- In Go, an array that can change size is called a slice.
You can iterate over an array or slice using range. -->
{{ $best_friends := slice "pumbaa" "timon" "nala" "rafiki" }}

<ul>
{{ range $best_friends }}
  <li>{{ . }}</li>
{{ end }}
</ul>

These are a few things that are possible with Templating.
There is a LOT more possible.
Best to look at the templating documentation to learn what else is possible.

The basic use of templating can be seen through adding a footer.
First add a parameter to the config.toml file.
Do this by writing a [params] object (for things on this site rather than special Hugo things)
and add my name:

[params]
name = 'Ethan Cowan'
Now this param will be available through the site.

Then we'll add the footer partial as a footer.html in /layouts/partials/
{{ with .Params.hide_footer }}
  <!-- No footer here! -->
{{ else }}
  <footer>
    Website made by {{ .Site.Params.name }} in {{ now.Year }}
  </footer>
{{ end }}

This is getting set up with some conditional logic using "with", so that if we want to hide the footer on one page, we have the option to do that by setting hide_footer: true in the front matter of any given page.

Then we call the partial before </body> in the baseof.html layout:
{{ partial "footer.html" . }} <---- again, there is the dot for the context of this particular page, tho I'm not sure if it's ever not needed... I'm a little fuzzy on how that works...

And then the footer shows up on the home page.
And then it's possible to hide the footer on the about page by setting hide_footer: true in the front matter under the title.
I'm curious if hide_footer is a built in param or if you can name any param you want...
So I'm going to try changing the name to hide_booter to see if it still works if it's named the same in both places...
And the answer is Yes, it does work even when it says hide_booter.
I think it works because it is set to a boolean value, True or False.
And I bet the conditionals work by just checking if a certain key is set to a certain value.
Doesn't matter what the key name is because the parser is checking the value, and doesn't care about the key.

Next is the creation of the blog!
2023-02-23 11:10am
