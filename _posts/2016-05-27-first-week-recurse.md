---
layout: post
title: My First Week At The Recurse Center
date: 2016-05-27 21:02:09 -0400
categories: recurse-center
---

I've just completed my first week at the [Recurse Center][recurse]! It's gone
by so fast, and I'm absolutely loving it so far. This is is a very carefuly
deesigned environment for learning, and I'm surrounded by passionate and
brilliant people. I've learned so much already, and I'm excited for the next 11
weeks.

[recurse]: https://recurse.com

This being the end of my first week, reflection seems apt. As you can see from
my [Wakatime][wakatime], I wrote about half of my code in Ruby (Ruby + ERB) and
about a third of it in Javascript, which is pretty ironic, because I only have
a cursory knowledge of the latter (I also used Emacs this week, but I only have
Wakatime installed in Neovim).

[wakatime]: https://wakatime.com/@fundirap

I've spent a lot of time working on the [Ruby on Rails
Tutorial][rails-tutorial] by Michael Hartl, and I'm glad to be almost done with
that. In the latter part of this week, I worked on a cryptography lab from
Stanford CS255, presented to the Recursers by Frank Wang, which challenged us
to build a library for a password manager in Javascript.

[rails-tutorial]: https://railstutorial.org/book

Last night was the first round of presentations for our batch. I was very
impressed by the work of other Recursers. The presentation of Emily Xie led me
to explore my interest in algorithmic art. Javascript seemed like a good
natural choice in a time where so much so media is delivered in browser, I got
to chatting with Charlie Brookhouse this morning, who suggested I look into
[p5.js][p5-js]. I've started playing around with it, and I accidentally made a
Spiernski triangle using only straight lines instead of filled triangles. You
can see my code below (humbly dubbed the `panashe_fractal`):

```javascript
function setup() {
  var container = document.getElementById("myContainer");
  var myCanvas = createCanvas(600,600);
  myCanvas.parent(container);
}

function draw() {
  background('white');
  stroke(0);
  noFill();
  panashe_fractal(width/2,height/2,width/4,height/4);
}

function panashe_fractal(x, y,xlen, ylen) {
  line(x-xlen/2,y,x+xlen/2,y);
  line(x-xlen,y-ylen/2,x-xlen,y+ylen/2);
  line(x+xlen,y-ylen/2,x+xlen,y+ylen/2);
  if (xlen > 2) {
    panashe_fractal(x+xlen/2,y,xlen/2,ylen/2);
    panashe_fractal(x-xlen/2,y,xlen/2,ylen/2);
    panashe_fractal(x+xlen/2,y,xlen/2,ylen/2);
    panashe_fractal(x,y-ylen/2,xlen/2,ylen/2);
  }
}
```

<a href="{{ site.url }}/assets/sierpinski.png"><img src="{{ site.url
}}/assets/sierpinski.png" alt="Sierpinski gadget" /></a>

[p5-js]: https://p5js.org

I spent most of my time here today (Friday) working on algorithmic
whiteboarding (interview preperation). I was prompted to produce an algorithm
that prints all of the paths through a binary tree that sum to a specific
value. Considering that this was my first time doing algorithms on a whiteboard
under pressure, I think I held my own. My facilitator, Kaley, and the other
Recursers who attended were also immensely helpful.

In reflection, I think I could have been bolder in asking for help with my
issues this week. The old cliché rings true: the only stupid question is an
unasked one, and Recurse is a very supportive environment for the curious mind.
I tend to try to wrap my own head around a problem before asking for help so I
can really grok the problem, but I can fall into the trap of banging my head
for hours without getting anywhere. This was inevitable when I worked alone,
but in such an environment this shouldn't be necessary. This is something that
I hope to rectify next week. I also could have done more planning around how I
used my time, but my life has been topsy turvy this week, and I'm sure that I
will be better able to plan my time as I become more settled.

Next week, I hope to be done with the Ruby on Rails tutorial, and be able to
build a good web app (such as the workout tracker I've been working on) with
the framework. I also hope to complete the password manager library for Frank's
crypto lab, and do some work on some [crypto pals][crypto-pals] problems. My
other goals for the week include generating some sexy algorithmic art with
p5.js, and get to hello world level in Rust.

[crypto-pals]: http://cryptopals.com

There are many things that I'm excited to do and work on in the near future.
I'd very much like to learn Haskell. Nabeil Hassein's enthusiasm for Idris
makes me want to check that out too, and if I like it, I may make some
meaningful contribution to the community (write a framework, maybe?) I've found
a group of other Recursers who I'll be studying Rust with. I don't know much
low level programming (a little bit of C/C++) so that should be exciting.
Ultimately, I'd be happy if I can write a small Unix shell.

On a non-Recurse/programming note, I need to get back into the gym ASAP. The
past three weeks have been rather tumultuous, so I hope I can finally put that
all behind me and resume my work towards my strength goals.

