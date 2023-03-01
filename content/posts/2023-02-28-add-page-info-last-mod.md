---
title: Last Mod feature
date: 2023-02-28T13:57:00Z
---

I made the [Now](/now) page, and I want it to have a _last updated_ feature.

I found a [blog post](https://makewithhugo.com/add-a-last-edited-date/) about how to do it.
Two options, a manual way and a git way.
I want to do the git way.

> Hugo can actually hook into your git (version control system) information and pull the last edited times from there. To enable it, just change this setting in your config.

config.toml
```
enableGitInfo = true
```

> This will now automatically pull n the last updated times and fill in `lastmod` for you - Neat!

There's a longer form to add to the config to change the front matter dates:

in config.toml
```
[frontmatter]
  lastmod = ["lastmod", ":git", "date", "publishDate"]
```

But then it doesn't work as I think when I add the code from the tutorial.
I need a refresher on how Templates work in Hugo, cause right now they're not evaluating, they're just printing on the post page.

This is a feeling of slight overwhelm, Why isn't it working?
It's a feeling I want to get more familiar with.

First thing I'll do is look at my other files that have templates that are working to see if I can find a mistake based on pattern matching.

Actually, one thing I want to do anyway is make this a partial, a reusable chunk of code that I can use on many pages, not just on the Now page.

So I'm going to go at it from that angle.
I did a similar thing with the footer element, so I'll use that as a model.

First, I'll create a new file called updated.html.
Important for me to remember, that's an html file.
I'll put the templates in there.
Then I'll add the template to the layout for single pages... Because that's kind of what I'm remembering.
The posts/pages themselves (the markdown files) are supposed to be "content in its purest form" and the logic and html stuff is supposed to happen in the layouts, I think.
Am I wrong?

So having saved updated.html with the following code:
```
<!-- Created Date -->
{{- $pubdate := .PublishDate.Format "02.01.2006" }}
created: 
<time datetime="{{ .PublishDate }}" title="{{ .PublishDate }}">
    {{ $pubdate }}
</time>

<!-- Last Updated Date -->
{{ if .Lastmod }}
    {{ $lastmod := .Lastmod.Format "02.01.2006" }}
    {{ if ne $lastmod $pubdate }} <!-- "ne" means "not equal" in Go -->
        <div class="post-info-last-mod">
            updated: 
            <time datetime="{{ .Lastmod }}" title="{{ .Lastmod }}">
                {{ $lastmod }}
            </time>
        </div>
    {{ end }}
{{ end }}
```

Next I'll test it by trying to add it to the default layout for single pages, called what?
Now I'm remembering... There's a baseof.html file, which is the basic layout...
Then there's also a single.html layout, which looks like this:

```
{{ define "main" }}
  {{ .Content }}
{{ end }}
```

single.html is where the main block is defined.
single.html just has the content in it... "content in it's purest form"
So I guess that means I should be adding updated.html to baseof.html and not to single.html.

Now here's what baseof.html looks like:
```
<!doctype html>
<html>
<head>
  {{ partial "meta.html" . }}
</head>

<body>
  {{ partial "header.html" . }}
  {{ partial "nav.html" . }}
  {{ block "main" . }}
  {{ end }}
  {{ partial "updated.html" . }}
  {{ partial "footer.html" . }}
</body>
</html>
```

So, when I add that updated.html as a partial to the baseof.html template, it works!
It's a little bit buggy, because some of the pages don't have a Publish date.
I wonder if only posts have a publish date...
Why don't the regular markdown pages, like Now and About, have a publish date?
I know that turning on enableGitInfo = true should allows hugo to get the lastmod info from git.

There's a line the tutorial had me add that read:
```
[frontmatter]
  lastmod = ["lastmod", ":git", "date", "publishDate"]
```
I think I need to understand more about what that line does.
Maybe I need to set publishDate to pull from git as well?

When I try to understand what's going on in that line of code, I don't understand why lastmod is being set to an array with those various strings in it...
But I see that's toml formatting...
In yaml it would look like
```
frontmatter:
  lastmod:
  - lastmod
  - :git
  - date
  - publishDate
```
Or maybe I need to have the updated.html partial pull from the the lastmod object instead of the .publishDate object...
Lemme try something.

After looking up the issue and finding answers on hugo discussion boards, the thing that fixes it is to make sure there is a valid date field in the front matter for every page.
Without the date field, hugo fills in "January 1, 0001"
I went through and added a date field, now all the pages have functioning information

Last thing I want to do is see if I can add a little bit of CSS to make the font smaller so this information is less conspicuous.
I used the css class selector "post-info-last-mod" to make the font .7em, same as the font in the footer.

Nice!
