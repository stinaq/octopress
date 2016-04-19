---
layout: post
title: "Setting focus on element programmatically in Angular on iOS"
date: 2016-04-19 16:50
comments: true
categories: 
- Angular
- jQuery
- JavaScript
- iOS
---

TLDR: Can't be done with `ng-click`, use jQuery or jQLite, or don't do it at all

Say that you have a web page that have a search bar at the top, but the search input is hidden from view when viewed from a small device. You have a button with a looking glass that when tapped it shows the input field. When the user want to search they don't want to first tap the button and then tap again to focus on the input field to have the keyboard appear, they expect this to happen at once. So one element is tapped, but another should get focused.

This is quite easily done in most browsers, but not those running on iOS. The iOS developers, preferring a stable and simple UI with little visual jank, where the keyboard does not show up unnecessarily, have ensured that input elements can’t be focused without the user initiating the process.

When does the keyboard appear in iOS?
---------------------------
First thing first, what are the ways the keyboard appears in iOS?

1. The natural way, the user taps an input element and the browser focuses that element and shows the keyboard
2. The programatic way, the user taps an element and a click handler sets the focus to a different element, but within what the iOS browser considers to be the same action.

Focus unsing a click handler
---------------------------
You can attach a jQuery click handler (probably also a native click handler, but I didn't try that) to an element and inside that set focus on an input field, and that will show the keyboard, like this:

```js
$('.wrapper').on('click', '#some-button', function () {
    $('#some-input').focus();
});
```

This will work in iOS and all other browsers that I've tried, since even iOS considers it user initiated. But if you remove the focus part from the timeline, for example putting it inside a timeout, it will no longer work in iOS, but other will happily set focus.

So this will *not* work:

```js
$('.wrapper').on('click', '#some-button', function () {
    setTimeout(function () {
        $('#some-input').focus();
    });
});
```

The call to `focus` has to be directly inside the click handler

But what about Angular?
-----------------------
This is where I had trouble. The application I'm writing on at work is all Angular, and you don't want to mix if you can help it. Mixing regular click handlers with `ng-click` makes for a hard to maintain application. 
I tried putting the exact same call to focus (using jQLite or jQuery, which ever) inside an `ng-click`, like this:

Template:
```html
<button id="some-button" ng-click="focusOnInput()">Click me</button>

```
In controller
```js
$scope.focusOnInput = function () {
    $('#some-input').focus();
};
```

But iOs couldn't have cared less. It would not work. I have no idea why, but for some reason iOS consideres this less user initiated, perhaps Angular itself does a timeout or something like it.
In the end I had to abandon `ng-click` in this case, even though it hurt somewhat, and use the same approach as above.

Looking around online I found others struggling with this. [Some had ways around](http://stackoverflow.com/questions/12204571/mobile-safari-javascript-focus-method-on-inputfield-only-works-with-click), some not, but this is all I could get to work.