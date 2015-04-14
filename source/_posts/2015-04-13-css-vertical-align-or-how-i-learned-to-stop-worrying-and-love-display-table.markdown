---
layout: post
title: "CSS Vertical Align or: How I Learned to Stop Worrying and Love Display: Table"
date: 2015-04-13 16:59
comments: true
categories: 
- CSS
- Display Table
- Positioning
---

I grew up with it and it's hard to shake. It follows me around, looking oven my shoulder everytime I try to position anything. That deeply ingained hatred of tables. I grew up and into web development hating it above all else. Nothing was as ugly and disgraceful as using tables to position content (that and maybe using capital letters for your tags). 

> - What, are you not using THE BOX MODEL? I can't work like this.

I sometimes sit and glare longingly at those at the office that don't have to support the older versions of IE. Imagine the glory and wonder of using Flexbox! To be able to roll around in ```align-items: center```, revel in ```justify-content``` and delight in ```flex width```. But, alas. It is not so, using flexbox for corporate oriented (especially SharePoint) styling is still some years off.

But I still have to verticle aligns things all the time. And this is why this post has come to life. Yes, it is true. I have started to use tables to get things to where I want them. Not actual tables, but virtual ones with div and span elements with display table. And I have come to, not love it, but rather like it. It sort of works, if you used correctly. There is a learning curve, especially steep one if your brain is telling you to RUN AWAY. But with some time and determination you too can get there.

The problem to solve
----------
<img src="https://dl.dropboxusercontent.com/u/16864288/blogg/verticle-align-with-table2.PNG">    

Typical is an icon that should be centered vertically to a text of unspecified height. ```Position: absolut``` won't help you here, neither margin top, since the height varies. What I used to do was using background image with a background-position: center. But these days we have to take retina screens into account. Begone blurry images! 

> But what about svg?

Says you.

Yes, that would also be nice, but legacy systems prevent me from using that (long story, SharePoint is to blame). Also I would like to utilize icon libraries like [FontAwesome](https://fortawesome.github.io/Font-Awesome/).

The solution
------------
Use table layout for this!
The wrapping parent gets:

```
display: table;
padding: 10px;
```

And then every child element gets 

```
display: table-cell;
vertical-align: middle;
```

And no padding on the children. Padded children messes everything up. Both child elements now becomes as high as the highest of them, and the content is places in the vertical middle. Magic. 

<img src="https://dl.dropboxusercontent.com/u/16864288/blogg/verticle-align-with-table1.PNG">

So you try this and there's no magic?
-------------------------------------
The problem with this methid is that it's mad hard to "debug". It's insanely difficult to understand what's wrong when something is off in the aligning. Try to remove all set height, line-height and margins and paddings on the child elements, as well as all other positioning styling (relative or absolute),