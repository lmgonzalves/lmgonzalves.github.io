---
layout: post
title: "Animated <span>refresh</span> icon"
date: 2016-02-02 16:00:00
categories: tutorial
comments: true
color: "#289E6B"
image: refresh.jpg
image_thumbnail: refresh.jpg
---

I would like to share a fun experimental animation I've done recently. It's about an animated refresh icon, based in [this dribbble shot](https://dribbble.com/shots/2488996-Refresh) by [Nick Buturishvili](https://dribbble.com/nick_buturishvili). I've made it using CSS animations, SVG and [segment](https://github.com/lmgonzalves/segment) mainly. But also I'm using the [d3-ease](https://github.com/d3/d3-ease) library for the `elastic-out` easing function used.

In general, this tutorial is about how to integrate successfully the segment library with CSS animations for creating beautiful and complex line animations. Maybe a collection of these kind of icons? Who knows ;)

<!--more-->

The truth is that I think segment has a great potential, and with some CSS (transitions and animations), I'm sure you can accomplish amazing animations, like this one:

<img src="{{ site.url }}/images/refresh-animated.gif" alt=""/>

### Drawing the paths

The first practical step to make these kind of animations is always the same: Drawing. You need to figure out how many paths do you need to perform the animation, considering not only the final state, but the whole animation.

In this particular case I'll be using only two paths. The first would be for the circular part, and the second for the arrow. This is the final code:

```html
<button class="refresh-icon">
    <svg viewBox="0 0 90 90">
        <path class="circle-path" d="M 45 45 m 0 -30 a 30 30 0 1 1 0 60 a 30 30 0 1 1 0 -60"></path>
    </svg>
    <svg class="arrow-svg" viewBox="0 0 90 90">
        <path class="arrow-path" d="M 50 15 m -18 -18 l 18 18 l -18 18"></path>
    </svg>
</button>
```

Note that I'm using differents `svg` for each `path`. That is because I'll be rotating the arrow with CSS, and I need it to make the rotation that I want. Also, Firefox 42 and below [do not support](http://caniuse.com/#search=transform) [`transform-origin` on SVG elements](https://bugzilla.mozilla.org/show_bug.cgi?id=923193).

### Animating the paths with segment

To animate paths with segment, first we need to initialize them:

```js
// Selecting elements
var refreshButton = document.querySelector('.refresh-icon');
var circlePath = refreshButton.querySelector('.circle-path');
var arrowPath = refreshButton.querySelector('.arrow-path');
var arrowSvg = refreshButton.querySelector('.arrow-svg');

// Initializing segments
var circleSegment = new Segment(circlePath, '25%', '90%');
var arrowSegment = new Segment(arrowPath, '25%', '75%');
```

Now we can animate the paths. A try and error process can be enough to get the correct values and timing:

```js
// Function that trigger the animation
function triggerAnimation(){
    // This perform the circular path animation, with an elastic ending
    circleSegment.draw('75%', '165%', 0.5, {circular: true, callback: function(){
        circleSegment.draw('10%', '90%', 0.2, {circular: true, callback: function(){
            circleSegment.draw('25%', '90%', 1.5, {circular: true, easing: d3_ease.easeElasticOut.ease, callback: function(){
            }});
        }});
    }});

    // Setting up a class to perform the CSS animation (arrow rotation)
    arrowSvg.setAttribute('class', 'arrow-svg arrow-animation');

    // This perform the arrow path animation, with an elastic ending
    arrowSegment.draw('50%', '50% + 0.01', 0.8, {callback: function(){
        arrowSegment.draw('25%', '75%', 1.4, {easing: d3_ease.easeElasticOut.ease, callback: function(){
            arrowSvg.setAttribute('class', 'arrow-svg');
            animating = false;
        }});
    }});
}
```

### Rotating the arrow with CSS animations

The CSS animation should rotate the arrow with a good enough timing, and then return to the starting point fastly. Let's have a look:

```css
.arrow-animation{
  animation-name: rotation;
  animation-duration: 0.8s;
  animation-timing-function: linear;
  animation-fill-mode: both;
}

@keyframes rotation{
  from{
    transform: rotate(-40deg);
  }
  80% {
    transform: rotate(340deg);
  }
  99.999%{
    transform: rotate(460deg);
  }
  to{
    transform: rotate(-40deg);
  }
}
```

### Conclusion

And that is all! We've made a beautiful icon, let's see:
<br/><br/>

<p data-height="190" data-theme-id="0" data-slug-hash="vLaXNR" data-default-tab="result" data-user="lmgonzalves" class='codepen'>See the Pen <a href='http://codepen.io/lmgonzalves/pen/vLaXNR/'>Fun Refresh Icon</a> by lmgonzalves (<a href='http://codepen.io/lmgonzalves'>@lmgonzalves</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Note that I've omitted some CSS and JavaScript code to focus only in relevant parts, but of course you can check the [full code on github](https://github.com/lmgonzalves/animated-refresh-icon). I really hope you like this tutorial and find it useful. You can also [play with the code](http://codepen.io/lmgonzalves/pen/vLaXNR) yourself. Happy coding ;)