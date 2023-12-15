---
layout: post
title:  "Notes on Jekyll and Github Pages"
date:   2023-12-14 20:38:17 -0500
---

# Done
* I set up a repository for my personal GitHub Pages site and activated Github Pages there (along [these lines](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)).
* I installed Jekyll (and Ruby, and bundler, and...) on my local machine (Windows 10 hosting Ubuntu in WSL).
  * Initially I didn't want to install Jekyll locally ("I should be able to just fill in a few things manually and have the [jekyll-build-pages](https://github.com/actions/jekyll-build-pages) GitHub Action do the rest")...
    * ... but this implied that I'd be "testing my changes in production", which is suboptimal for a series of small changes (the action takes ~30s to run) and also *kinda stressful on a philosophical level*.
  * Instead, because I jumped through the hoops, I can see my changes roughly in real-time with ```bundle exec jekyll serve --livereload```, which seems much more civilized.
* I created this post!

# To Do
* Surface topics and lists of posts about said topics.
  * A word cloud or sorted list of the most common topics would be cool.
    * Look for plugins that do this?
  * How to specify the topics?
    * Jekyll posts can have [tags and categories](https://jekyllrb.com/docs/posts/#tags-and-categories).
      * Categories affect a post's URL. For example:
        * The [default "Welcome to Jekyll!" post](/jekyll/update/2023/10/30/welcome-to-jekyll.html)'s categories are ```jekyll``` and ```update```.
        * Its URL has a path that starts with ```/jekyll/update```
      * Navigating directly to ```/jekyll/``` yields an unformatted directory listing with an entry for ```update/```
        * The order in which categories are specified matters
          * different orders -> different locations in the hierarchy
          * ```categories: jekyll update``` goes in ```/jekyll/update```
          * ```categories: update jekyll``` goes in ```/update/jekyll```
      * This view would be more ergonomic if it recursively displayed headers and snippets for the posts in the category.
        * Can this be changed?

* Make local dependencies match what GitHub Pages is using.

  The default Gemfile created by ```jekyll new``` says:
  ```
  # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
  # uncomment the line below. To upgrade, run `bundle update github-pages`.
  ```

  It may just be that simple.

* Dark mode. 'Nuff said.

* Remove the default "Welcome to Jekyll!" post.

* Update the _config.yml boilerplate (blog title, description, GitHub link, About page, etc.)

# Resources
* I used the GitHub documentation for [Quickstart for GitHub Pages](https://docs.github.com/en/pages/quickstart) as a jumping-off point.
* Jekyll's [default markdown](https://jekyllrb.com/docs/configuration/markdown/) is GitHub Flavored Markdown (GFM), which is documented [here](https://github.github.com/gfm/).
  * The markdown-HTML mappings are neat.
  * I wish they had inline "here's what it looks like" snippets.
