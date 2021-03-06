---
layout: post
title: "Fluid video embeds for your responsive website"
---
Recently I came across an interesting problem, fluidly scaling iframe/embed code on a responsive design.

The problem surfaced because I was using a fluid width and a fixed height for my video embed, and so at some screen sizes the video would look fine, and at others it would have some black banding at the edges.

<!-- more -->

![](http://uk.omg.li/NaZt/by%20default%202013-03-15%20at%2015.52.00.png)

Not very good. So a solution was needed.

If you can bare to not just use the iframe code YouTube gives you for embedding video, then you can use this solution.

1. Wrap the video in a container
2. Give the container a fluid width
3. Add vertical padding to the container to get the correct aspect ratio
4. Make the video 100% width and height
5. Position everything

![](http://uk.omg.li/NaiM/by%20default%202013-03-15%20at%2015.53.50.png)

Here's an example:

<pre><code data-language="css">.emedded-video-wrapper {
    position: relative;

    /* 100% width, so it scales */
    width: 100%;

    /* and this is where the magic happens */
    height: 0;
    padding-bottom: 56.25%; /* a magic number! */
}

.embedded-video-wrapper > * {
    position: absolute;
    top: 0;
    left: 0;

    /* make the iframe/embed fill the wrapper */
    height: 100%;
    width: 100%;
}
</code></pre>

You might notice that I've used a `magic number` in there, `56.25%` -- it's not that magic actually.. For a `16:9` aspect ratio video, `9` is `56.25%` of `16`.

Here's a lookup table for the various standard video aspect ratios:
- `4:3 : 75%      (SD)`
- `16:9 : 56.25%   (HD)`
- `1.85:1 : 54.05%   (Cinema)`

Taking it one step further, to make this easily reusable, you could create classes for each of the aspect ratios:

<pre><code data-language="css">.emedded-video-wrapper {
    position: relative;

    width: 100%;
    height: 0;
}

.embedded-video-wrapper > * {
    position: absolute;
    top: 0;
    left: 0;

    height: 100%;
    width: 100%;
}

/* aspect ratio classes */
.sd {
    padding-bottom: 75%;
}
.hd {
    padding-bottom: 56.25%;
}
.cinema {
    padding-bottom: 41.84%;
}
</code></pre>

And then you'd use the following `HTML`, the only adjustment from the code that YouTube provide is that I've wrapped it in a div, and removed the width/height properties (though it should work fine if you leave them as-is, because the CSS will override them)

<pre><code data-language="html">&lt;div class="embedded-video-wrapper hd">
    &lt;iframe
        src="http://www.youtube.com/embed/xEhaVhta7sI"
        frameborder="0"
        allowfullscreen
    >&lt;/iframe>
&lt;/div></code></pre>

You could give your `.embedded-video-wrapper` any width you like now, and the video height will scale proportionately.

