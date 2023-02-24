---
title: Skeleton from Scratch
date: 2023-02-23T05:39:00Z
---

5:39am

If I do the quickstart instructions from the Hugo page, I end up with a site that is visible on the localhost immediately.
So that means the file system it initializes is 'complete' in some sense of the word.

On the other hand, if I initialize my own site, the file system skeleton is there, but it's relatively empty, and nothing is visible on local host immediately.
"No page found" when I start the development server.

So what makes the difference?
The quickstart site shows it has 7 pages, 1 static file and 1 sitemap.
The from-scratch site shows it has 3 pages, no static files and 1 sitemap.

In the from scratch site, there are 2 files I can see.
default.md is in the archetypes folder
config.toml sits in the root directory ('test' directory)
all the other skeletal folders are empty
If those two files are 'pages,' then I wonder what is the third page?
If those two files are not pages, then what are the three pages listed on the summary?

In the quickstart site, there are more files.
Still have default.md in the archetypes folder
Still have config.toml in roote directory ('quickstart' directory)
Now there are two documents deep in the "resources" directory.
resources > _gen > assets > css > ananke > css > main.css....content
resources > _gen > assets > css > ananke > css > main.css....json
AND
there is an entire THEME in the themes directory
This theme is called ananke, and it has it's own skeletal structure with significant meat on the bones.
It has an folder called 'example site' that contains content.
It has layouts.
It has a bunch of stuff.

But I don't think all of that stuff is being run when I start the hugo development server because all I see when I start the server is an empty layout with 'My New Site" titles, etc.

I'm not sure where these html files are.
I don't think they're inside the Theme folder because the theme folder has a bunch more stuff in it that I'm not seeing on the development page.
I don't think I'm seeing the example site.

What I do see is a README.md and that's actually a key because it has instructions about getting the full example site going.
What I don't see is a full working site.
It's empty of content.
It's just the frame, or the layout?
So I'm not sure if that's because I'm doing something wrong or if the setup automation doesn't run entirely smoothly.

Either way, after poking around the readme for a little bit, I'm ready to go back to trying to build a site from the skeleton up.
I think I'll learn more that way about how it all works.

I'm going to use a tutorial/walkthrough that's linked on the hugo docs, the hugo tutorial by CloudCannon.
I'm going to call my new site lowerpower.
I thought of it this morning when I was lying in bed feeling my lower abdomen, thinking about maturity, and thinking about the ubiquitous 12 step phrase "Higher Power"

The CloudCannon walkthrough gives a nice overview of the scaffolding or skeleton of the site.
It gives brief but informative sentences about what each directory is for.

Then part 2 explains that "a layout is used for all the 'framing' on a page around the content."
Header, footer, menus.

"In Hugo, each page on the site is a content file.
At the top is a small snippet of metadata called front matter, followed by markdown.
The goal of the content file is, well, to store the content in its purest form.
There's rarely any HTML or other presentation logic in a content file.

All of the fancy HTML to display and format the content lives in a layout.
One layout might be used for multiple content pages.
Other times, particularly if you're doing something intricate, you might have a layout specifically for an individual content page."

"There's a Hugo concept called Page Bundles which can be tricky to get your head around.
We're going to do the bare minimum here to get your site going.

By default, content pages use a layout called single.
Creating a content file which is named exactly _index.md, underscore and all, is the exception to this.
This page acts like a table of contents for the pages around it and uses a layout called list by default.

So...
Create a file called _index.md in the directory /content/ with the following content:
---
title: Home
---
The home page

Okay, so the first piece of content... what happens when I run the development server now?
I still get some of the same warnings I got before:
WARN 2023/02/23 06:34:17 found no layout file for "HTML" for kind "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN 2023/02/23 06:34:17 found no layout file for "HTML" for kind "taxonomy": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN 2023/02/23 06:34:17 found no layout file for "HTML" for kind "taxonomy": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.

So, I can see that when hugo builds the site, it's looking for layout files and actually those layout files are required for the build to run smoothly or complete.
I still get "page not found" in the browser.

So I have to make a layout

"Hugo has a hierarchy to figure out which layout to use for a content_d file.
The last fallback is looking for a layout in the _default directory.

So... create a new directory inside /layouts/ called _default and inside, create list.html with the following content:

<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>{{ .Page.Title }}</title>
</head>

<body>
  {{ .Content }}
</body>
</html>

Okay, so that is a very bare bones HTML 'layout'
This is the default list.html layout which hugo will use as the layout for _index.md.
So when hugo build the website, it will:
1. see _index.md in the content directory
2. look for a layout file in the _default directory
3. find the list.html layout (used for index pages, and what else?) in the layout directory
4. build an HTML file by putting the content into the layout
	(this is why there are "{{" "}}" - those contain the content. The content is a variable being passed into a layout function)

And voila, the content shows up on the development server now :)
There is a title that shows up in the tab "Home"
And there is content on the page "The home page"

Okay, now the tutorial goes on to a second layout, an about page
Create /content/about.md (note that there is no underscore - that means the underscore on _index.md is an important symbol to the hugo program, probably a symbol that helps the program identify where to start when it's first running through (parsing?) the content of all the files).

Or is the underscore an indication of something else?
Maybe it tells the program which layout to look for?
Earlier warnings were about not finding a layout for kind "home" or for kind "taxonomy" 
Did that have to do with the underscore?
What is a *kind*?

Now if I just try to run the build with this second about.md file, what will happen?
Will it build?

Ethans-MacBook-Air:lowerpower dreamer$ hugo serve
Start building sites … 
hugo v0.110.0+extended darwin/amd64 BuildDate=unknown
WARN 2023/02/23 06:54:37 found no layout file for "HTML" for kind "page": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.

Another warning!
So the about.md page looks for a different *kind* of layout, a layout for kind "page"
A different function, not the list.html function.

about.md looks for a layout named 'single' by default.

"We could clone /layouts/_default/list.html to /layouts/_default/single.html and it would work fine
BUT
You'd be left maintaining two very similar versions of the same layout.
We want to reduce repetition and make our lives easier wherever possible.

Instead create a new layout at /layouts/_default/baseof.html with the following content:

<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>{{ .Page.Title }}</title>
</head>

<body>
  {{ block "main" . }}
  {{ end }}
</body>
</html>

Okay, so baseof.html sounds like it can be the building block of other kinds of layouts?
"This is almost identical to the list layout, with one minor change. 
Instead of outputting {{ .Content }}, we now have a main ‘block’. 
This block is a placeholder that other layouts can use to only specify that part of the page."

Okay, this seems important.
A block is a placeholder that other layouts can use to only specify that part of the page.
So this being baseof.html makes more sense when I think that this has a placeholder for other layouts to use.
The syntax of {{ block "main" . }} seems important.
How does this work?
How are other layouts going to define "main"?
What is the "." doing?

And this isn't enough to get the about page built.
I still get the same error about no layout file for "HTML" for kind "page"

But now the next step of the tutorial explains I need to update the list.html layout so that it can play nicely with this new baseof.html layout

"Replace the entire contents of /layouts/_default/list.html with the following:
{{ define "main" }}
  {{ .Content }}
{{ end }}

Okay, so now I see how a layout block is defined.
use {{ define "..." }}
	{{ .Content }}
    {{ end }}

Now the baseof.html file will have something nice to handle when it tries to execute that function to put the main block into the body of the html doc.
I still don't know what the "." means exactly.

What happens if I try to build now?
I think I'll still get the warning about the page layout because all I've done is reset the list.html.
Probably the next step is to make the html layout for the kind "page"

Yep, Create /layouts/_default/single.html with the same code as list.html and
Voila!

Now when I run the build, it throws no warnings and I can see both the index and the about page in the broswer!
Cool!

So, to review:
When hugo builds, it seems like these are the steps:
1. Look for content
2. When content is found, look for a layout (function) to put that "content in its purest form (markdown)" into an HTML structure
3. If it's an index file (with an underscore), look for layouts for kinds "home" and "taxonomy"
4. If it's a regular md file, look for a layout for kind "page"
5. If those elements are found, then put the markdown into the html layouts!

And I'm seeing these things on the development server (which is nice), but I also notice that I do NOT see anything in /public/ which means that hugo hasn't actually BUILT anything yet.
It has some temporary html stuff it's showing, but no actually html files written into the public directory yet.

2023-02-23 7:20am
