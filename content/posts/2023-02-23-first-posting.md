---
title: Adding Posts
date: 2023-02-23T11:10:00Z
---

Finally I'll get to the point where I can add these blog posts to the blog.
I am expecting this step to be very gratifying.
I am also expecting it to include lots of reformatting, because I've been writing all of this stuff as plain text and I'll need to make it all into MarkDown.
I'm excited about MarkDown because it's still so close to plain text, and I think it can be opened as plain text (and all of this because I liked Derek Sivers point about writing in plain text), but MarkDown will have some formatting options.

And once there is content on the blog, it will be possible to play with it.
I will be able to play with the formatting, play with the CSS and the styling.
Play with table of contents stuff...
There will be stuff to play with :)

So here we go, making the blog.
First, make a directory called posts in the /content/ directory
then make a file inside posts called _index.md with the following:
---
title: Blog
---

"_index.md - remember what that means? 
It's a table of contents file, and in this case it will be listing the posts.

At this point, it's going to try to use the /layouts/_defualts/list.html layout.

But you don't want to use this layout as this page doesn't have any content (and we want the content to come from a template that will list the posts).
So we want a new layout specifically for listing posts.

The way the Hugo layout hierarchy works is it will first look for alayout that matches the current section/directory, then fall back to the global default in _default.
In other words, we can create a new directory called posts in the layouts directory, and inside create "list.html" with the following content:

{{ define "main" }}
  <h1>My posts</h1>
  <ul>
  {{ range .Pages }}
    <li>
      <a href="{{ .Permalink }}">{{ .Title }}</a> - {{ .Date.Format "January 2, 2006" }}
    </li>
  {{ end }}
  </ul>
{{ end }}

There are a few new concepts in this piece of code:
1. A list page (_index.html) has an array of all its children pages with the variable .Pages.
2. .Date has format called on it and gets passed a random date in 2006. Why is that? It’s a quirk of Go for formatting dates. You can read more about it here.
3. .Permalink can be called on any page to get its end URL. It’s particularly useful if you want to link to a page.
That’s all we need for our list page. Let’s move onto a post.

NOW, Finally... Posts!
The posts live in the /content/posts/ directory and don't require an special naming convention.
One tip the tutorial suggests is adding the date of the post to the file name.
This just helps with finding stuff later.

So, now I'll add my 8 posts, including this one :)
Well, I'll start with three, set up the stuff, then see how things look!

Just like the blog list page, these posts will try to use /layouts/_default/single.html.
But we can create a layout specifically for posts at /layouts/posts/single.html.

{{ define "main" }}
  <h1>{{ .Params.Title }}</h1>
  <p>{{ .Date.Format "January 2, 2006" }}</p>
  {{ .Content }}
{{ end }}

Finally, I'll add the blog to the navigation.
Open /layouts/partials/nav.html
Add another list item:
<li><a href="/posts/">Blog</a></li>

And voila, all the posts are showing up on the blog page :)
Now I'll spend some time adding the other 5 posts...
12:04pm
