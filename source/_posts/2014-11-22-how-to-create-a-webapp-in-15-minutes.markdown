---
layout: post
title: "How to create a webapp in 15 minutes"
date: 2014-11-22 10:00
comments: true
categories: 
- Geek Girl Meetup
- Tutorial
---

If you came to this post because we met at the Geek Girl Meetup, I'm so, so glad to see you. My goal was to inspire at least one person to actually try this out and get started with creating webapps by using Yeoman. It would make me feel really warm inside that I actually inspired someone to get started and try it out.

If you just stumbled in here by yourself, this is a post where I list the few and wonderously simple steps to generate a webapp to use for web development using Yeoman.

The steps
========

Install nodejs
--------------
Go to [nodejs.org](http://nodejs.org) to download Node. Install it on your computer.

Test Node Package manager
-------------------------
Open your terminal (if you're on a Mac), or your Power Shell if you're on a Windows. Don't know how to do this? Don't worry, [here is a great guide for Mac](http://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line), and [here is a great guide for Windows](http://technet.microsoft.com/en-us/library/hh847889.aspx). You don't need to know more than how to open it, and think of how cool you'll look now!

With it open, type in

```
npm
```

Press enter. That's it. You get a response that says stuff about how npm works. Moving on to the next step!

Install Git
-----------
Git needs to be installed in order to get some other things (Bower) working. Go to [http://git-scm.com/downloads](http://git-scm.com/downloads), download and install.

Install Yeoman
--------------
[Yeoman](http://yeoman.io) is a program that helps you generate skeletons for your future web page or web application. Install it by typing this into your terminal/Power Shell:

```
npm install -g yo
```

This installs Yeoman on your computer (the -g part makes sure it's installed everwhere). If you want to know more about how Yeoman works, [this is a good place to start](http://yeoman.io/learning/index.html).

Install Bower
-------------
[Bower](http://bower.io) helps you out with dependencies such as jQuery or Bootstrap or something else. Install it by typing:

```
npm install -g bower
```

Interested in what you can do with Bower, visit [this guide](http://code.tutsplus.com/tutorials/meet-bower-a-package-manager-for-the-web--net-27774).

Install Grunt
-------------
[Grunt](http://gruntjs.com) is what you call a "task runner". It does tasks for you, like minifying CSS and JavaScript, or compiling Sass, or copying files from one place to another. Let's install it by typing:

```
npm install -g grunt-cli
```

If you want more about what Grunt can do for you, go [this guide](http://www.smashingmagazine.com/2013/10/29/get-up-running-grunt/), which explains it nicely.

Install Webapp generator
------------------------
Now we're at the last installation step, which is to install one of Yeomans' generators, namely [Webapp](https://github.com/yeoman/generator-webapp). It's a generator that creates a bunch of files and folders containing all you need for your future project. Type

```
npm install -g generator-webapp
```

Webapp is the most general generator from Yeoman, but there are a [bunch of others](http://yeoman.io/generators/).

Create project folder
---------------------
Now it's time to create a folder where your project will live. Either create it in finder/explorer, or look cool by doing it in your teminal/Power Shell:

```
mkdir my-fancy-project
```

Then you have to move your terminal/Poweer Shell to your new folder. This is done by the command ```cd``` which stands for 'change directory'. Type in:

```
cd my-fancy-project
```

Generate your project
---------------------
Now the time has come for a project to be created, finally! And all you have to do is type:

```
yo webapp
```
And after you pressed enter all the files and folders and content will be created, it's almost like magic.

Start up your web application in the browser
--------------------------------------------
Once it's finished, type

```
grunt serve
```
This starts up the task manager Grunt, which opens your browser, with the web site on it.

Then what?
---------
So that's all you need to do, now you can start creating. That's not always easy, but it's loads more fun than writing boring HTML and creating folders and so on, which all these steps have done for you.

I made the [Counting Down site](http://www.countingdown.nu) by using a generator from Yeoman. You can add some JavaScript to the generated JavaScript file to create something that tells time, or just says hello, or shows flash cards from your anatomy class at school, or something entirely different.

It would make me happy if you mailed me at stinaqv@gmail.com with questions, tales or thoughts about this. I hope that you try it out and see where it takes you.