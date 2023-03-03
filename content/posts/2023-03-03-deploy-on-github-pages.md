---
title: Deploy on Github Pages
date: 2023-03-03T13:28:00Z
---

I'm gonna deploy on github pages because I want to see if I can make a podcast feed and I _think_ I need to be deployed to do it.

Following instructions on [Hugo docs](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

login to github

create a new repository called 'elocowan.github.io'

push the existing lowerpower project file to that repository from the command line

```
git remote add origin https://github.com/elocowan/elocowan.github.io.git
git branch -M main
git push -u origin main
```

But I run into an authentication problem. 

remote: Support for password authentication was removed on August 13, 2021.

remote: Please see [this doc](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls) for information on currently recommended modes of authentication.

So i could use a personal access token.

Or, another option seems to be using [GitHub CLI](https://docs.github.com/en/github-cli/github-cli/quickstart).
That seems like the better option so I'll do that.

install GitHub CLI
```
brew install gh
```

then use gh to authenticate my GitHub account
```
gh auth login
```
Follow the instructions for that authentication, then it's all working.

so when I push to the main branch of that new repository, it works.

Now all the stuff on my computer is also in the github repository.
That doesn't mean that I see anything when I go to elocowan.github.io

The next step was a bit confusing. 
The instructions say

> Create a file in .github/workflows/gh-pages.yml containing the following content (based on actions-hugo):
```
name: github pages

on:
  push:
    branches:
      - main  # Set a branch that will trigger a deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

What's confusing is "create a file in .github/workflows/gh-pages.yml"
I guess that means in the project directory?
I make a .github director with a workflows directory inside it.
Then I open gh-pages.yml in vim, and paste that stuff in.
Save it.
Will that do the trick?

Now I will push those changes to the git respository.
```
git add .
git commit
git push
```

All the changes, including the yaml file, show up in my github repository, but still nothing showing at elocowan.github.io.

I think it's because of this:
> the GitHub Actions in these instructions publish to the gh-pages branch. Therefore, if you are publishing GitHub pages for a user or organization, you will need to change the publishing branch to gh-pages. See the instructions later in this document.

I think that means that Hugo is setup to publish to gh-branches... and that branch doesn't even exist on my github repository.

I wonder, if I run the `hugo` command, will it make that branch?
