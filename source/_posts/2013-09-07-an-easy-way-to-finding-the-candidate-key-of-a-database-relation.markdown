---
layout: post
title: "An easy way to finding the Candidate Key of a database relation"
date: 2012-11-21 20:50
comments: true
categories: 
- Database
- Normal form
- Canditate key
- Tutorial
---


<h2>A beginner's guide to locating the Candidate Key and Normal Form of a relation.</h2>
There are many <a href="http://www.koffeinhaltig.com/fds/kandidatenschluessel.php">systems</a> and algorithms that can help you find the candidate key of a particular relation. What these don't tell you is how much easier this can be if you just learn to draw appropriate boxes and arrows! Granted, you might not look like the coolest kid in a group of hardened database programmers if you do this. But we are all beginners in the, you know..beginning.

Swallow your pride, this is a really good and clear way! This article is meant for those of you who are starting out with relational databases and normalization. I hope you know the purpose of normalization and what the different normal forms are.

Every relation has one or more candidate keys. A candidate key (CK) is one or more attribute(s) in a relation with which you can determine all the attributes in the relation. Later on, when you implement the relation in a database, the candidate key, or one of the keys, will be the <strong>primary key</strong> of a relation. Knowing the candidate key is also needed to determine the normal form of the relation. But how do you find it? Well, you can either look all professional and use algorithms. Or you can draw it!

&nbsp;
<h4>An example of how to draw you way to the candidate key</h4>
Let me show you with an example. This relation deals with movies and actors.

Movie(title, year, genre, genre-description, direcor, director-home-country)

First, we have to list the dependencies. Which attributes are dependent on which? An arrow means that by knowing the one to the left, you know the one to the right.

title --&gt; year

title --&gt; genre

title --&gt; director

genre --&gt; genre-description

director --&gt; director-home-country

So what I do is that I make a box for each of the attributes and then simply add arrows to represent dependencies.

{% photo /images/custom/candidate-key1.png /images/custom/candidate-key1_s.png Find the candidate key %}

Now it's extremely easy to locate the least amount of attributes you need to determine the rest of the relation: <strong>Title</strong>! No arrows are pointing at title and all the other boxes have arrows that originate from title. Therefore: <strong>The candidate key of this relation is Title</strong>, and that makes title a primary attribute (an attribute that is part of  candidate key). More on primary attributes later, when we continue on to deciding the normal form. This example was pretty basic and simple, let's continue on to something a bit trickier. And let's also leave this with attributes that makes sense behind; my imagination was stretched a litte too thin by the last example... Consider the relation:

R(A, B, C, D, E)

where

A --&gt; B

B  --&gt; A

B --&gt; C

D --&gt; A

Then you'll get this drawing:

{% photo /images/custom/candidate-key2.jpg /images/custom/candidate-key2_s.jpg Find the candidate key %}

No arrow is pointing at neither D or E, so in order to determine all the attributes in this relation you'll need to know both D and E, since nothing can determine either of them. This gives you a composite key. C<strong>andidate key: {D, E}.</strong> C and E together. I'll show just one more example before moving on to using this knowledge. A trickier and larger one.

R(A, B, C, D, E, F, G)

A --&gt; B

B  --&gt; {A, C, E}

C --&gt; {B, F, D}

F --&gt; {D, G}

The curly brackets are just a short hand. Instead of saying A --&gt; B, and A --&gt; C, you write A --&gt; {B, C}. 
{% photo /images/custom/candidate-key3.png /images/custom/candidate-key3_s.png Find the candidate key %}

Here you actually have several candidate keys. If you know A you can determine the entire relation, but that is also true for B and C. From B you can get to all the other boxes by just following arrows, the same for C and A! So the result is <strong>several candidate keys, A, B and C.</strong>

&nbsp;
<h3>Using the drawing to determine the normal form of a relation</h3>
The drawings are also a great help on the way to find out which normal form a relation has. All you have to do is know how to draw boxes, arrows and what the rules of the <a href="http://en.wikipedia.org/wiki/Database_normalization#Normal_forms" target="_blank">normal forms say</a>.

So let's try it with a relation.

R(A, B, C, D)

A --&gt; B

B --&gt; C

C --&gt; {B, D}

Which gives this drawing:

{% photo /images/custom/normal-form1.png /images/custom/normal-form1_s.png Find the normal form %}

The Candidate Key of this relation is A, since that's all you need to determine the rest of the relation. That makes A the only primary attribute (an attribute that is part of a candidate key). But what of the normal forms? I will go through them one by one.
<ul>
	<li><strong>First normal form</strong>. <span style="text-decoration: underline;">Are all the attributes atomical</span>? Well, <strong>yes</strong>! None of them are more than one, so to speak.</li>
	<li><strong>Second normal form</strong>. <span style="text-decoration: underline;">Is there a non primary attribute that's determined by parts of any candidate key?</span> This implies that we need a composite candidate key (one that consist of two or more attributes, like in the second example above), and since we don't, this can't be true and the <strong>relations is therefore in 2nd normal form</strong>. To have a relation that is not in 2nd normal form, there would have to be an arrow pointing <em>from</em> an attribute that is part of a candidate key, <em>towards</em> an attribute that is not part of a candidate key.</li>
	<li><strong>Third normal form</strong>. <span style="text-decoration: underline;">Does a non primary attribute exists that's determined by another non primary attribute?</span> Yes, in this case there is! There is an arrow from B to C and both of them are non primary attributes, so <strong>this relation is <span style="text-decoration: underline;">not</span> in 3rd normal form</strong>.</li>
</ul>
What about 4th and Boyce Codd and all the other normal forms, then? Well, a relation that's in 4th normal form must fulfill the rules regarding the 4th normal form as well as all the others "leading up" to it. So, if it breaks the rule for 3rd, it can't be in 4th.

One last example.

R(A, B, C, D)

A --&gt; {B, D}

B --&gt; {A, C}

C --&gt; B

The drawing:

{% photo /images/custom/normal-form2.png /images/custom/normal-form2_s.png Find the normal form %}

And the candidate key will be A, B and C.
<ul>
	<li><strong>First normal form</strong>. Are all the attributes atomical? Still yes.</li>
	<li><strong>Second normal form</strong>. Is there a non primary attribute that's determined by parts of a candidate key? No, the same as above. The only non primary attribute is D and it's not determined by parts of a candidate key.</li>
	<li><strong>Third normal form</strong>. Does a non primary attribute exists that is determined by another non primary attribute? No, no two non primary attributes determine each other. In this case we only have one, so it can't happen.</li>
	<li><strong>Boyce-Codd normal form</strong>. <span style="text-decoration: underline;">Are all the deteminates candidate keys?</span> Or, as I like to think of it, are all attributes to the left of the arrows candidate keys? You just look at where the dependencies are listed (above the drawing) and see if all those to the left are candidate keys. In this case, they are.</li>
	<li><strong>Fourth normal form</strong>. Deals with the problems that can arise from having multivalued dependencies, and seeing as there are no multivalued dependencies, we can assume that it is good to go for the 4NF!</li>
</ul>
So, the relation is in 1 to 4th normal form and Boyce-Codd, and we'll leave the rest for another time! I hope this article has made the topic of candidate keys and normal forms somewhat clearer. There are probably many other ways to solve this problem, but this has worked great for me.

Feel very free to contact me with questions or suggestions.

/SQ