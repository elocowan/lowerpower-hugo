---
title: Fix GitHub Deployment
date: 2023-03-04T16:00:00Z
---

My deployment isn't working like it should.
It should be updating my live site anytime I push to GitHub.
But I don't have things configured right.

I'm going to try using [this blog post](https://dev.to/aormsby/how-to-set-up-a-hugo-site-on-github-pages-with-git-submodules-106p) as a model for how to set up my deployment.

1. Create repository at https://github.com/elocowan/lowerpower-hugo.git

From within /lowerpower/
```
git remote add origin https://github.com/elocowan/lowerpower-hugo.git
git branch -M main
git push -u origin main
```
2. Create repository for public files at https://github.com/elocowan/elocowan.github.io.git

From within /lowerpower/
```
git submodule add -b master https://github.com/elocowan/elocowan.github.io.git public
```

3.
