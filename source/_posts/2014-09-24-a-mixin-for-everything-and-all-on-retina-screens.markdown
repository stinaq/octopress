---
layout: post
title: "A mixin for everything and all on retina screens"
date: 2014-09-24 22:06
comments: true
categories:
- CSS
- SASS
- Code snippets
---

*Problem:* You have styles you want for regular screens, and styles you want for retina screens. Mostly images with different sizes, but sometimes font sizes or widths.

I was faced with this recently and got annoyed at the amount of repeating I had to do. First it's the fuck ugly syntax of the media queries, then it's the browser compatibility, and then I have to repeat it all over my Sass document for different screen sizes and everything else that can change

Queue trying to build a handy mixin for it all
--------------------------------------
This is what I came up with:

    @mixin retinaspecific($maxwidth, $attribute, $value) {
        @media
            only screen and (-webkit-min-device-pixel-ratio: 2) and (max-width: $maxwidth),
            only screen and ( min--moz-device-pixel-ratio: 2) and (max-width: $maxwidth),
            only screen and ( -o-min-device-pixel-ratio: 2/1) and (max-width: $maxwidth),
            only screen and ( min-device-pixel-ratio: 2) and (max-width: $maxwidth),
            only screen and ( min-resolution: 192dpi) and (max-width: $maxwidth),
            only screen and ( min-resolution: 2dppx) and (max-width: $maxwidth) {
            #{$attribute}: $value;
        }
    }

It's a mixin that takes three arguments. The first ($maxwidth) handles the actual media query and when the rule should be applied. The second ($attribute) handles what *attribute* is affected. Here you put font-size, background-image or something like it. And the last one handles the value of the attribute, so what size should the font be, or what background image do you want.

It is quite handy! Just use it whenever you want something styled for a retina screen. Since you can reuse it for all attributes and screen sizes, it shourtens the code you have to write quite a lot.

Using it like this:

    @include retinaspecific(500px, min-width, 55px);

Will compile to this:

    @media 
        only screen and (-webkit-min-device-pixel-ratio: 2) and (max-width: 500px),
        only screen and (min--moz-device-pixel-ratio: 2) and (max-width: 500px),
        only screen and (-o-min-device-pixel-ratio: 2 / 1) and (max-width: 500px),
        only screen and (min-device-pixel-ratio: 2) and (max-width: 500px),
        only screen and (min-resolution: 192dpi) and (max-width: 500px),
        only screen and (min-resolution: 2dppx) and (max-width: 500px) {
            min-width: 55px;
    }

I used it for my app [Counting Down](http://countingdown.nu) and as far as I know it doesn't work in LESS