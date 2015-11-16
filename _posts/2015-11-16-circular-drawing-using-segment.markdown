---
layout: post
title: "<span>Circular</span> drawing using Segment"
date: 2015-11-16 08:00:00
categories: tutorial
comments: true
color: "#DE593A"
image: circle.jpg
image_thumbnail: circle.jpg
---

Today I'm very happy to release a new featured coded inside Segment. From now, you'll could animate the paths in a "circular" way.
This means that, for example, if you set an `end` value greater than the path length, when the stroke reaches the end of the path it will resume at the beginning. The same applies in the opposite direction.

This also means that you can use negative values to define `begin` and `end` values (can be percentages), besides higher values.
To work in this way you need to set the new option `circular` to `true`. For example:

```js
segment.draw("-175%", "-125%", 1, {circular: true});
```

<!--more-->

Here you have a little demo to show the potential of this new feature:
<br/><br/>

<p data-height="262" data-theme-id="0" data-slug-hash="rOPeQP" data-default-tab="result" data-user="lmgonzalves" class='codepen'>See the Pen <a href='http://codepen.io/lmgonzalves/pen/rOPeQP/'>Circular drawing using segment</a> by lmgonzalves (<a href='http://codepen.io/lmgonzalves'>@lmgonzalves</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

I really hope you like it and find it useful. Happy coding ;)