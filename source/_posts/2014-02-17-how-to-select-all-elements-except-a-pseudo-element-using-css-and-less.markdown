---
layout: post
title: "How to select all elements except a pseudo element using CSS and LESS"
date: 2014-02-17 14:12
comments: true
categories: 
- CSS
- LESS
- Tutorial
---

This may not be the easiest way to achieve this results, but I wanted to share it because it was genuinely awesome. 

I wanted to put a bottom margin on all elements, except the last one, and I wanted to do it using nested LESS-syntax. Sure, I could just write two rules, one that put the bottom margin on all elements, and one that removed it on the last child. Like this:

    .content {
      margin-bottom: 30px;
      &:last-child {
        margin-bottom: 0;
      }
    }
  
But where's the fun in that? No learning = no fun.

I couldn't wrap my head around it, so I turned to StackOverflow and got exactly the kind of [answer I wanted](http://stackoverflow.com/questions/20631212/less-nested-rule-for-selecting-all-except-a-pseudo-element). User [BoltClock](http://stackoverflow.com/users/106224/boltclock) explained and here's what I learned:

What I wanted to do was to select all elements that were children of `.content`, but not the last one, so I had to select all children with the child selector `>` , then single out the last one with `:last-child` and put it in the `:not`-selector. Like this:

    .content {
      > :not(:last-child) {
        margin-bottom: 30px;
      }
    }
    
Which in its turn compiled to:

    .content > :not(:last-child) {
      margin-bottom: 30px;
    }
    
In the end I had to abandon it because there was an IE8 requirement in place. IE8 causes so much sadness.
