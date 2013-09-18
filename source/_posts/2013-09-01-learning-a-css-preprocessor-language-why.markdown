---
layout: post
title: "Learning a CSS preprocessor language. Why?"
date: 2013-07-13 21:15
comments: false
categories: 
- Things that annoy me
- Excitement
---

The important question here is: Can you get by without it?

Here’s how it usually plays out when you take your first steps into the world of web styling: You start out with learning CSS, setting colors, height of boxes. Getting annoyed with margins not behaving as they should.  Creating classes, using IDs, getting annoyed with specificity. Trying out gridsystems, using hover and getting annoyed with  float. Actually, a lot of CSS creates some itchy feelings. In my opinion, it is too simple. There’s too much manual labour, too high a risk for mistakes.

CSS preprocessor languages are, for me, a way to make it bearable. Here follows a short introduction to [LESS](http://lesscss.org/), a preprocessor language that I recently looked at at work. The other big player in the field is [Sass](http://sass-lang.com/), a comparison of the two will feature in a future post. This is a really short and simple summary of what Less can do

Top 3 things that LESS can do:

## Variables

I’ve missed these, how did I ever live without them! Imagine you have a site with a fixed width, say for example 970px. You write 970px over and over again all over your stylesheet. And then… you change your mind. You really don’t like 970px. 980px is the way to go. Now you have to switch the two values all over the document, and find-and-replace is an awful idea, cause some of the element still want to be 970px. It is soul killing, mind numbing work. What if you could just change it in one place? Use a LESS variable!

``` css LESS variable
@the-width-of-stuff: 980px;
```
Now it is as simple as using the variable in your LESS-stylesheet.

## Mixins

This is a way to create a rule that you can then use and reuse in another rule. This way you don’t have to write the code for creating a browser resistant gradient over and over again. You can just use the same bit of CSS. Mixins can be created with or without parameters. If you want the exact same code again, you don’t need parameters, but say for example you want different colors on your gradient, LESS lets you provide arguments to your mixin!

#### Mixin without parameters:

Here is a mixin called .backgroundcolormaker that when used, sets the background color to red. Now it contains just one attribute, but it can ofcourse be expanded just as a normal CSS rule.

``` css Basic mixin
.backgroundcolormaker {
   background-color: red;
}

.some-class {
   width: 980px;
   .backgroundcolormaker;
}
```

Will output this:

``` css Basic mixin - output
.some-class {
   width: 980px;
   background-color:red;
}
```

#### Mixin WITH parameters:

Here is a mixin that takes one argument. It sets the given value to the background-color attribute. This way you can use the same rule but with different values. You can specify more than one argument when creating a mixin.

``` css Mixin with parameter
.backgroundcolormaker (@bg-color) {
   background-color: @bg-color;
}

.some-class {
   width: 980px;
   .backgroundcolormaker(pink);
}
```

Will output this:

``` css Mixin with parameter - output
.some-class {
   width: 980px;
   background-color:pink;
}
```

## Libraries

With both LESS and Sass you can import libraries with mixins to use. Finished, usable snippets that help with browser support and complex CSS3 attributes. For SASS the most popular one is [Compass](http://compass-style.org/), for Less there are [LESSHat](http://lesshat.com/), [Bootstrap](http://bootstrap.lesscss.ru/less.html) and [Elements](http://lesselements.com/), among others. For example you can use the .transform mixin to transform an element with your prefered values, without having to worry about browser compatibility. Or the [border-radius](http://lesshat.com/#transform) one that creates nice, rounded corners for you. Extremely practical!

 

Do yourself a service and take a look at a CSS preprocessor language of your choosing. I know I can’t live without it now.

I will be back soon with integration with Visual Studio and a comparison of LESS and Sass!