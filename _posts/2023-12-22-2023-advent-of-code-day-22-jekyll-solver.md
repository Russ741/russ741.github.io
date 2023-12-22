---
layout: post
title:  "A Visual Solver For Day 22 of 2023 Advent Of Code"
date:   2023-12-22 15:47:00 -0500
tags:   threejs advent-of-code
---

# Building, Blogging, Showing, Solving[^le-carre]

This is an experiment where I try to:
* write a solver for the [Day 22: Sand Slabs](https://adventofcode.com/2023/day/22) puzzle of this year's [Advent of Code](https://adventofcode.com/2023/about) in JavaScript...
* ... and visualize the puzzle using [three.js](https://github.com/mrdoob/three.js/#readme)...
* ... while concurrently writing a blog using Jekyll that documents the *process* and presents the results...
* ... as well as providing selected insights into Jekyll that *facilitated its own creation*.

# Pre-work: Set up Jekyll

See [Notes On Jekyll And GitHub Pages]({% post_url 2023-12-14-notes-on-jekyll-and-github-pages %}) [^how-to-link] [^how-to-footnote] for some details about how I set up this installation and pointers to further documentation.

# Create a new post

Create a new markdown file (like [this one](https://github.com/Russ741/russ741.github.io/blob/main/{{page.path}}?plain=1)) under the ```_posts``` directory.

Optional: Run a Jekyll server locally to preview it: ```bundle exec jekyll serve --livereload```

# Footnotes and Acknowledgements
[^le-carre]:
    With apologies to [John Le Carré](https://en.wikipedia.org/wiki/Tinker_Tailor_Soldier_Spy)

[^how-to-link]:
    Thank you to Michael Rose's [URLs and links in Jekyll](https://mademistakes.com/mastering-jekyll/how-to-link/#-post_url--and--link--tags) for drawing my attention to the [post_url tag](https://jekyllrb.com/docs/liquid/tags/#linking-to-posts).
    * Note that the reference to the post does **not** include the file extension.

[^how-to-footnote]:
    Thank you to Jake Lee's [An introduction to GitHub & Jekyll's footnote functionality, and finding its limits](https://blog.jakelee.co.uk/footnote-experiments-on-github-and-jekyll/#supported-contents) for helping me to get footnotes working.
    * Note that for multiline footnotes (i.e. including bulleted lists like this one to work), it helps to:
        * put the footnote tag ```[^how-to-footnote]:``` on its own line and
        * indent the contents of the footnote (by however many spaces the rest of the file uses; I use four)

[^how-to-page-path]
    Thank you to [mb21](https://stackoverflow.com/users/214446/mb21) for [mentioning](https://stackoverflow.com/a/13300410) the page.path [page variable](https://jekyllrb.com/docs/variables/#page-variables) that I used in the GitHub markdown link.