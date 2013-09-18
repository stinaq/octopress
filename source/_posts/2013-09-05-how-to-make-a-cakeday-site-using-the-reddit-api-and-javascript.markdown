---
layout: post
title: "How to make a cakeday site using the Reddit api and JavaScript"
date: 2013-02-21 22:37
comments: true
categories: 
- JavaScript
- REST
- API
- Reddit
- Cakeday 
---

This is a short guide on how to use the Reddit API to create a cakeday-site, for beginners. A cakeday is the day someone joined the community Reddit, so it's like an online birthday. My final version is called <a href="http://tellmemycakeday.com/">tellmemycakeday.com</a>, but this guide will create a small version of that one.

If you are having trouble seeing how the code looks in its entirety, it can be found at the bottom of the post.

My mind was set to try out JavaScript. The time hade come, and I wanted to do something useful. Well, something you could use. Well, something that did something. So why not create a page that tells you your Reddit cakeday? It's been done before, but what hasn't? So I thought I'd share how I did it so that everyone can try, 'cause it was fun. It's so quick and nimble, results come fast and you get amazed at the possibilities.

<h4>Getting the data</h4>
The first step is to figure out how to get hold of a specific user's account creation date. What's great about Reddit is that all their data is available via a public <a href="http://en.wikipedia.org/wiki/Application_programming_interface">API</a>. What that means is that they are openly showing how their site works, and that anyone can create a Reddit application just by looking at their <a href="http://www.reddit.com/dev/api">public and open source code</a>. The part we're going to use is about <a href="http://www.reddit.com/dev/api#section_users">the users</a>.

The lookup is done via an AJAX call to Reddit. What Reddit wants is a string that contains information on which user and how you want the data. It looks like this:

<em>http://www.reddit.com/user/someuser/about.json?jsonp=?</em>

The next thing we need is a JavaScript file to execute our code. I'm using jQuery, and this is all the code needed to fetch the right response and print it in the console:

```javascript
$(function(){
    $.getJSON('http://www.reddit.com/user/someuser/about.json?jsonp=?',
    function(data){
        console.log(data);
    });
});
 
```

Put it in a file with the ending .js. This makes a GET-request for the user named "someuser" and then prints that user in the console (the console.log(data)-part).

Create an HTML-file containing the following:

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
        <script type="text/javascript" src="app.js"></script>
        <title>Cakeday</title>
    </head>
    <body>
    </body>
</html>
```

The first script tag links to a CDN hosting jQuery, the second to your own, new JavaScript file. If you now open your HTML-file in a browser (I'll assume you use Chrome), you can see the exact response! Right-click, choose "inspect element" and open the console (to the right).

Here is what I saw:

{% photo /images/custom/response.png /images/custom/response.png Reddit response %}

All the data is there! You can see how much link and comment karma this user has, if it is a mod and, what's important to us, when it was created. Now it is a matter of using this data in a creative way.

<h3>Formatting the date</h3>
So, the date 12011825216 is not very readable. It's the date formatted as <a href="http://en.wikipedia.org/wiki/Unix_time">UNIX time</a>, the number of seconds since the 1st of January 1970. We need to change it into something prettier! And I used <a href="http://momentjs.com/">Moment.js</a> to do just that. By just utilizing that small library the number is transformed into something a human can understand.

```javascript

var createdDate = moment.unix(data.data.created);

```

This creates a manageble date that can then be used to figure out how many days are left until the next cakeday. The part inside the parenthesis is the date we got from Reddit, and moment.unix tells Moment.js that it is a date in unix-format.

The next part can be a bit difficult for someone who, like me, is slightly mathematically challenged. How do you find out how many days there are left? The created-date is sometime in the past, all you need is the day and month. So I replaced the year in the createdDate with the current year. Unless, off course, your cakeday has already happened this year, then we need the next year.

We put this bit of code inside a function that takes the date the user was created as parameters and returns the number of days left to the next cakeday. I think the code is more readable and managable when divided into neat functions.

``` javascript
var caluclateDaysLeft = function(createdDate) {
    var currentYear = moment().format('YYYY');

    var nextCakeday = createdDate.year(currentYear);

    if(nextCakeday < moment()){
        nextCakeday = nextCakeday.add('years', 1)
    }
return nextCakeday.diff(moment(), 'days');
}
```

<h3>Showing the days left to the user</h3>
Now we have calculated the date and can view it in the console. What is the next step? Well, instead of displaying it in the console we want to put it in the browser window. This is done by manipulating the HTML, using jQuery. jQuery can help us select elements in the HTML and insert out information.

What we want is to display the days left to the visitor, and to do this we need to add an element to the previously created index.html.

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
        <script type="text/javascript" src="moment.min.js"></script>
        <script type="text/javascript" src="app.js"></script>
        <title>Cakeday</title>
    </head>
    <body>
        <p id="result"></p>
    </body>
</html>
```
We've added a link to the Moment.js-file and a paragraph to hold the result.

In our JavaScript-file we add a line that puts the number of days into the paragraph.

```javascript
$('result').text(daysLeft);
```

It finds the element with the id "result" and then sets the text to what we found before.
<h3>Getting a username from the visitor</h3>
The site is not very usable if you can't choose which username you want to look up. How to create this behaviour? We need to create a form that can take input from the visitor and then use that input in our JavaScript.

First, we modify the index.html to look like this:
```html
<!DOCTYPE html>
<html>
    <head>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
        <script type="text/javascript" src="moment.min.js"></script>
        <script type="text/javascript" src="app.js"></script>
        <title>Cakeday</title>
    </head>
    <body>
        <form id="cakedayForm">
            <input type="text" placeholder="Your username" id="userName">
            <br />
            <button type="submit" class="btn btn-success">Submit</button>
        </form>
        <p id="result"></p>
    </body>
</html>
</form>
```

That adds a form with an input field and a submit button. This is all we need!

In the JavaScript we need to grab the text from the input-field and run the function when the user presses the button. The code below is triggered when the "submit"-event happens, which is exactly what we want. It takes the value written by the user and then runs the function that displays the days left to that particular user's cakeday.

```javascript
$(document).on("submit", '#cakedayForm', function(event) {
    event.preventDefault();
    var user = $("#userName").val();
    displayDaysLeft(user);
});
```

So now we actually have all the parts needed to have a site that takes a Reddit username and displays how many days there are left to that users's cakeday!
<h3>Putting it all together</h3>
All we need to do is put it all together, like this:

``` javascript 
$(function(){
    $(document).on("submit", '#cakedayForm', function(event) {
        event.preventDefault();
        var user = $("#userName").val();
        displayDaysLeft(user);
    });

    var displayDaysLeft = function(user) {
        user = $("#userName").val();
        var userUrl = "http://www.reddit.com/user/" + user + "/about.json?jsonp=?";

        $.getJSON(userUrl, function(data){
            var createdDate = moment.unix(data.data.created);
            var daysLeft = caluclateDaysLeft(createdDate);
            $('#result').text(daysLeft);
        });
    };

    var caluclateDaysLeft = function(createdDate) {
        var currentYear = moment().format('YYYY');
        var nextCakeday = createdDate.year(currentYear);

        if(nextCakeday &lt; moment()){
            nextCakeday = nextCakeday.add('years', 1)
        }
        return nextCakeday.diff(moment(), 'days');
    };
});
```

This does not, however, take into account what to do when the user doen't exist, or if the visitor doesn't write anything in the input, but that's left as an exercise to the reader. And the site also looks awful! The code can be <a href="https://github.com/stinaq/cakeday/tree/master/tinyversion">found on Github</a> and a live version is running on <a href="http://tellmemycakeday.com/tinyversion">tellmemycakeday.com/tinyversion</a>. And if you want to have a look at the extended version, prettier and with error handling, you can find it on <a href="http://tellmemycakeday.com">tellmemycakeday.com</a>, this is also <a href="https://github.com/stinaq/cakeday">avaliable on github!</a>