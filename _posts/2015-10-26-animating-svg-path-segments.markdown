---
layout: post
title:  "Animating SVG path <span>segments</span>"
date:   2015-10-26 18:10:00
categories: tutorial
comments: true
image: path.jpg
image_thumbnail: path.jpg
demo: http://lmgonzalves.github.io/segment/
code: https://github.com/lmgonzalves/segment
---

I have been working on a tool that allows to animate the SVG `path` strokes in a configurable way. Because the only tool that
I know to perform this kind of work is [DrawSVGPlugin](http://greensock.com/docs/#/HTML5/GSAP/Plugins/DrawSVGPlugin/) from
GSAP, but unfortunately it is not free.

For this reason I have been figuring out how to animate the `path` stroke with the attributes `stroke-dasharray` and
`stroke-dashoffset`, and the result is the tool that today I put in your hands:

<!--more-->

> **Segment**, a little JavaScript class (less than 2kb, without dependencies) to draw and animate SVG `path` strokes.

You can check the [demo](http://lmgonzalves.github.io/segment/), or get the [source code](https://github.com/lmgonzalves/segment)
from GitHub. Additionally, you may be interested in understanding how it works, in that case, I'll be happy to show you the theory behind.

### Basic knowledge

First you need to understand the [stroke-dasharray](https://developer.mozilla.org/en/docs/Web/SVG/Attribute/stroke-dasharray)
and [stroke-dashoffset](https://developer.mozilla.org/en/docs/Web/SVG/Attribute/stroke-dashoffset) attributes.
I also recommend you [this excellent article by Jake Archibald](https://jakearchibald.com/2013/animated-line-drawing-svg/).

So, how to draw a `path` stroke from a certain distance to another using these attributes?

### Drawing from `begin` to `end`

The answer is to define the attributes in the following way:

```
stroke-dasharray = "totalLength, begin, end - begin"
stroke-dashoffset = "totalLength"
```

<p data-height="310" data-theme-id="0" data-slug-hash="qOoVyZ" data-default-tab="result" data-user="lmgonzalves" class='codepen'>See the Pen <a href='http://codepen.io/lmgonzalves/pen/qOoVyZ/'>Drawing SVG segments</a> by lmgonzalves (<a href='http://codepen.io/lmgonzalves'>@lmgonzalves</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Since `stroke-dasharray` specify the `lengths` of alternating dashes and gaps, the first value (`totalLength`) means
that the `length` of the first dash will be equal to the total `length` of the `path`. However, as we have defined the
same `length` for `stroke-dashoffset`, this dash will not be visible because it is "pulling" to the beginning of the `path`.

The next value (`begin`) is the `length` of the first gap, which ends where you actually want to start drawing. Finally we have
defined a third value (`end - begin`), which corresponds to the `length` of the next dash, being drawn in this way only
the part between `begin` and `end`.

Having defined three values (odd) for `stroke-dasharray`, it is equivalent to repeat
those values to yield an even number of them. Then the next gap will have a `length` equal to the total `length` of the
`path` (first defined value).

This is a basic formula for drawing the segment you want of a `path`.

### Why to use Segment?

The advantages of using [Segment](https://github.com/lmgonzalves/segment) is that you can animate the transition from one
segment to another, define a `begin` and `end` values with expressions like `"50% + 10"`, set delays, easing functions,
callbacks, and all these with less than 2kb!

That's all (for now). I hope it has been useful, your comments are welcome. And don't forget to share with your friends :)
