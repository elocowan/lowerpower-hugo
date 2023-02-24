---
title: Setting Up Hugo
date: 2023-02-22T13:15:00Z
---

1:15
Starting to set up hugo.
First thing I notice when I go to the documentation page is that the first thing they mention in their overview is "Hugo's Security Model".
Why is security the first thing?

"When developing and building your site, the runtime is the hugo executable.
Securing a runtime can be a real challenge."

What is a runtime?
A runtime is when the program runs.
In the case of Hugo, it's when Hugo builds the website... it makes the static HTML output.
The program is the hugo program that 'does stuff' to make the website happen.

I think that the problem is, the files for Hugo projects are located in the filesystem of the computer, and the build requires Hugo to open some passage between the file system and some HTTP something... or is it passage between the file system and a slew of uncheckable dependencies that are part of the Hugo program?

Or the problem is in all of the dependencies that are part of the build process.
There have been high visibility cases of malicious software within dependencies.

The point is that we need ways to keep malicious code from accessing the contents of certain files, getting secrets or sensitive information from a users file system.

"Hugo's main approach is that of sandboxing and a security policy with strict defaults:
- Hugo has a virtual file system
	- only the main project (not third party components) is allowed to mount directories or files outside the project root.
- Only the main project can walk symbolic links
- User-defined components have read-only access to the filesystem

The thing is, they want to show that they are taking precautions to protect users against code injection.
I'm not super worried about that, but maybe I should be?

Anyway, for me it's a lot of work to understand a single page of this Hugo documentation.

Some questions that come up:
I know that hugo is built with the Go language.
What more do I need to know about Go?

How much do I need to GET before I just install Hugo and get started making a website?


Then there's a section about the General Data Protection Regulation (GDPR), which is law in the EU and seems good everywhere.

"Hugo is a static site generator.
By using Hugo you are standing on very solid ground.
Static HTML files on disk are much easier to reason about compared to server and database driven web sites.

But even static websites can integrate with external services..."

So there are privacy settings related to disabling tracking by other websites like disqus, googleAnalytics, instagram, etc.


Then, with those security and privacy STUFF out of the way, What is Hugo?

"Hugo is a static site generator.
Unlike systems that dynamically build a page with each vistor request, Hugo builds pages when you create or update your content.
Since websites are viewed more often than they are edited, Hugo is designed to provide an optimal viewing experience for your website's end users
AND
an ideal writing experience for website authors."

I notice they don't say website architects or coders or engineers.


"What does Hugo do?
In technical terms, Hugo takes a source directory of files and templates and uses these as input to create a complete website."

So Hugo is a big function.
It takes input of files and templates
DOES STUFF
then outputs a complete website.

"Who should use hugo?
Hugo is for people that prefer writing in a text editor over a browser. (That's me!)
Hugo is for people who want to hand code their own website without worrying about setting up complicated runtimes, dependencies and databases.
Hugo is for people building a blog, a company site, a portfolio site, documentation, a single landing page, or a website with thousands of pages."

Implication is that Hugo is NOT for people who want a dynamic website with user inputs... More for websites to be read than websites to be edited/interacted with. (I think).


Features that pique my interest:
"taxonomies including categories and tags
sort content as you desire through powerful template functions
automatic table of contents generation
dynamic menu creation"

And probably more stuff that I don't yet even realize I want.

"The purpose of website generators is to render content into HTML files.

When you make a request to an HTTP server (a server using the Hyper Text Transfer Protocol), the server responds to the request by sending HTML files to the browser to be viewed.

On dynamic sites, the code to generate the HTML files lives in a database somewhere, and when a user requests the HTML files, the server starts a process to create a new HTML file every time an end user requests a page.

That takes time.
In order to save time, "dynamic site generators were programmed to cache their HTML files to make things faster.
A cached HTML file is a static HTML file."

"Hugo takes caching a step further and all HTML files are rendered on your computer."
SO THAT IS WHAT HUGO DOES
Hugo is a big function that takes my files and templates
DOES STUFF
then outputs HTML files all organized correctly to function as a website.

The HTML files are on my local machine.
I copy the HTML files to the computer that hosts the HTTP server.
So when people make a request to the server for my HTML files, there they are, ready to go.
They are static and simple.

HTTP servers are very good at sending files.
So it's fast.

So what is happening when I "install hugo" is that I'm downloading the Hugo function so I can use it to make HTML files.

Prerequisites
"Although not required in all cases, Git and Go are often used when working with Hugo."

So first, I need to download Go.
I do want to learn more about Go, but right now I just want to download it.

Go is a language, so I'm putting in on my computer so that I can install Hugo.

I could download the binary code for Hugo and extract the archive, move the executable to the desired directory, add the directory to the PATH environment variable, verify I have execute permission on the file, then execute the binary to install that way....

BUT, I'm just going to use Homebrew, a package manager for macOS

Then I'll use the suggested quickstart, to "learn to create a Hugo site in minutes."

2:25pm

2:29pm
Quickstart site is up and running.
Super quick start.
I have questions about what it means to have a development server going.
I'm pretty sure it means that the Hugo program makes it so that I can edit files on the computer and see immediate results in the broswer.
I'm at a URL called localhost:1313.
I don't think I even need to be connected to the internet.
It would be fun to look under the hood to see what is going on when the Hugo development server is running.
Maybe that's for another time...

Cool, so when you run the hugo command 'hugo', it builds the site.
Or as they say, it 'publishes' the site.
It fills up the public file in the project directory with the HTML files that reflect the latest changes.

It's cool to go through and look at what gets made.
What is an xml file?
	It says in the docs that xml files are "RSS feeds" for each section
What is all the extra code that ends up in the index.html file?

It says in the documentation, "Hugo does not clear the public directory before building your site.
Manually clear the contents of the public directory before each build to remove draft, expired, and future content."

The next part in the documentation is Modules, and I'm pretty cooked from working through this much content, so this post will end here.

Next time, I'll find out, What is a module?
