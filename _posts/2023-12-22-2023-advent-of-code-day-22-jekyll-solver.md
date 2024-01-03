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

Create a new markdown file (like [this one](https://github.com/Russ741/russ741.github.io/blob/main/{{page.path}}?plain=1) [^how-to-page-path]) under the ```_posts``` directory.

Optional: Run a Jekyll server locally to preview it: ```bundle exec jekyll serve --livereload```

# Add a HTML canvas holder, a canvas, and draw something
References: MDN - [Basic usage of canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Basic_usage#a_simple_example) and *Jekyll with Three.js* [^three-js-jekyll]

I've decided to create the ```canvas``` element dynamically in JS rather than with HTML to mimic adding the [renderer's domElement](https://threejs.org/docs/#api/en/renderers/WebGLRenderer.domElement).

#### Code

```html
<div id="canvas-holder">
</div>

<script>
    canvasHolder = document.getElementById('canvas-holder');
    canvas = document.createElement('canvas');

    /* Add a border to visualize where the canvas is */
    canvas.style.border = "1px solid black";

    /* Draw something in the canvas */
    ctx = canvas.getContext("2d");
    ctx.fillStyle = "rgb(200, 0, 0)";
    ctx.fillRect(10, 10, 100, 100);

    /* Insert the canvas into the document */
    canvasHolder.appendChild(canvas);
</script>
```

#### Demo

<div id="canvas-holder1">
</div>

<script>
    canvasHolder = document.getElementById('canvas-holder1');
    canvas = document.createElement('canvas');

    /* Add a border to visualize where the canvas is */
    canvas.style.border = "1px solid black";

    /* Draw something to the canvas */
    ctx = canvas.getContext("2d");
    ctx.fillStyle = "rgb(200, 0, 0)";
    ctx.fillRect(10, 10, 100, 100);

    /* Insert the canvas into the document */
    canvasHolder.appendChild(canvas);
</script>

# Load three.js, insert its canvas, and draw something
References: MDN [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), three.js [Creating a scene](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene)

#### Code

```html
```

#### Demo
<script type="importmap">
    {
        "imports": {
            "three": "https://unpkg.com/three/build/three.module.js"
        }
    }
</script>

<div id="canvas-holder2">
</div>
<br />
<textarea id="puzzle-input" cols="80" rows="10">
1,0,1~1,2,1
0,0,2~2,0,2
0,2,3~2,2,3
0,0,4~0,2,4
2,0,5~2,2,5
0,1,6~2,1,6
1,1,8~1,1,9
</textarea>

<script type="module">
    import * as THREE from 'three';

    class Brick {
        xyzL;
        xyzH;

        constructor(inputLine) {
            const [x1, y1, z1, x2, y2, z2] = inputLine.match(/\d+/g).map((val) => parseInt(val));
            this.xyzL = new THREE.Vector3(x1, y1, z1);
            this.xyzH = new THREE.Vector3(x2, y2, z2);
            if (z1 > z2) {
                [this.xyzL, this.xyzH] = [this.xyzH, this.xyzL];
            }
        }

        get l() {
            return this.xyzH.x - this.xyzL.x + 1;
        }

        get w() {
            return this.xyzH.y - this.xyzL.y + 1;
        }

        get h() {
            return this.xyzH.z - this.xyzL.z + 1;
        }

        get middle() {
            return v3(1, 1, 1).add(this.xyzH).add(this.xyzL).divideScalar(2);
        }
    }

    function loadInput() {
        const input = document.getElementById('puzzle-input').value;
        const inputLines = input.split('\n').filter((line) => line.length > 0);
        const bricks = inputLines.map((inputLine) => new Brick(inputLine));
        return bricks;
    }

    function getLine(p1, p2, color) {
        const material = new THREE.LineBasicMaterial({color});
        const geometry = new THREE.BufferGeometry().setFromPoints([p1, p2]);
        return new THREE.Line(geometry, material);
    }

    function v3(x, y, z) {
        return new THREE.Vector3(x, y, z);
    }

    canvasHolder = document.getElementById('canvas-holder2');
    canvasHolder.style.border = "1px solid black";
    /* TODO: Resize the whole canvas when the window's resized. */
    const width = canvasHolder.clientWidth;
    const height = width;

    const scene = new THREE.Scene();
    const bricks = loadInput();
    for (const brick of bricks) {
        const prism = new THREE.Mesh(
            new THREE.BoxGeometry(brick.l, brick.w, brick.h),
            new THREE.MeshNormalMaterial());
        prism.position.copy(brick.middle);
        scene.add(prism);
    }

    scene.add(getLine(v3(0, 0, 0), v3(10, 0, 0), 0xFF0000));
    scene.add(getLine(v3(0, 0, 0), v3(0, 10, 0), 0x00FF00));
    scene.add(getLine(v3(0, 0, 0), v3(0, 0, 10), 0x0000FF));

    const camera = new THREE.PerspectiveCamera( 75, width / height, 0.1, 1000 );
    camera.position.x = 5;
    camera.position.y = 5;
    camera.position.z = 5;
    camera.lookAt(0,0,0);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(width, width);
    renderer.render(scene, camera);
    canvasHolder.appendChild(renderer.domElement);
</script>

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

[^how-to-page-path]:
    Thank you to [mb21](https://stackoverflow.com/users/214446/mb21) for [mentioning](https://stackoverflow.com/a/13300410) the page.path [page variable](https://jekyllrb.com/docs/variables/#page-variables) that I used in the GitHub markdown link.

[^three-js-jekyll]:
    Thank you to Long Qian's [Jekyll with Three.js](https://longqian.me/2017/02/06/jekyll-threejs/) (source code [here](https://github.com/qian256/qian256.github.io/blob/master/_posts/2017-02-06-jekyll-threejs.md?plain=1)) for demonstrating how to embed three.js in a Jekyll post.
