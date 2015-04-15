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

I grew up with it and it's hard to shake. It follows me around, looking over my shoulder everytime I try to position anything. That deeply ingrained hatred of tables. Nothing was as ugly and disgraceful as using tables to position content (that and maybe using capital letters for your tags). 

> - What, are you not using THE BOX MODEL? I can't work like this.

I sometimes sit and glare longingly at those at the office that don't have to support the older versions of IE. Imagine the glory and wonder of using Flexbox! To be able to roll around in ```align-items: center```, revel in ```justify-content``` and delight in ```flex width```. But, alas. It is not so, using flexbox for corporate oriented (especially SharePoint) styling is still some years off.

But I still have to vertically align things all the time. And that's the reason for this post. Yes, it is true. I have started to use tables to get things to where I want them. Not actual tables, but virtual ones with div and span elements with display table. And I have come to, maybe not love it, but at least rather like it. It sort of works, if used correctly. There is a learning curve, an especially steep one if your brain is telling you to RUN AWAY. But with some time and determination you too can get there.

The problem to solve
----------
<img src="https://dl.dropboxusercontent.com/u/16864288/blogg/verticle-align-with-table2.PNG">    

Typically we're talking about an icon that should be centered vertically to a text of unspecified height. ```Position: absolute``` won't help you here, nor will margin top, since the height varies. What I used to do was use a background image with background-position: center. But these days we have to take retina screens into account. Begone blurry images! 

> But what about svg?

says you.

Yes, that would also be nice, but legacy systems prevent me from using that (long story, SharePoint is to blame). Also I would like to utilize icon libraries like [FontAwesome](https://fortawesome.github.io/Font-Awesome/).

The solution
------------
Use table layout for this!
The wrapping parent gets:

```
display: table;
padding: 10px; // For good looks
```

And then every child element gets 

```
display: table-cell;
vertical-align: middle;
```

And no padding or margin top or bottom on the children, that messes everything up. Both child elements now becomes as high as the highest of them, and the content is placed in the vertical middle. Magic. 

<img src="https://dl.dropboxusercontent.com/u/16864288/blogg/verticle-align-with-table1.PNG">

Want a clearer picture and try it out yourself? [Here's a plunker where you can do that](http://plnkr.co/edit/t319jK5ThJ3mDPtoQ6ak?p=preview)

So you tried this and there's no magic?
-------------------------------------
The problem with this method is that it's mad hard to "debug". It's insanely difficult to understand what's wrong when something is off in the aligning. Try to remove all set height, line-height and margins and paddings on the child elements, as well as all other positioning styling (relative or absolute).

Nothing new under the sun
-------------------------
Yes, people have of course used this before, but that doesn't mean everyone has, or that it isn't news to someone. After all, until recently I didn't use it and I spend a fairly large amount of my day arguing with CSS.