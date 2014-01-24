---
layout: post
title:  "First steps Angular.js with D3"
date:   2014-01-23 09:41
categories: tutorial angularjs d3  
---

This post is inspired on the great talk that [Victor Powell][url-victor-powell] did in the [Angular.JS Announcements, January 2014][announcements2014] , check out his great website and materials. 

These first steps assume that you know about [Angular.js Directives][angularjs-directives-doc], so check them out if you don't understand them. And now, time to follow the steps of Victor Powell to learn how to unfold all [D3][url-d3] (a promising Data-Driven Documents visualization library) potential with Angular.js directives.

So let's follow Victor's steps and start learning about this. With directives we can customize our HTML tags, for instance like the `<input>` field which creates a textfield to type in our text. With D3 and directives we can think of having a tag like `<donut-chart></donut-chart>` and in the same way that `<input>` works its way, we don't need to worry about how the donut chart is going to be drawn. 

First of all, let's see how to draw a D3 donut chart without using AngularJS yet, we just write our D3 code and call if from a `<div class="donut-chart"></div>`: 

{% gist 8575714 %}

Of course, don't forget to write the CSS code for the donut-chart class:

{% gist 8575754 %}

###Moving on with AngularJS

We type our `donut-chart` directive telling AngularJS we want it to be an Element. This directive will use the link function (as usual) and it this function that is going to add the `svg` code into our view. We can now move our D3 code inside the `link` function and instead of selecting the `div` from the DOM, we get the element 0 from `el`. We will need to change our CSS code and make `.donut-chart` class into a selector. 

{% gist 8576234 %}

Cool, now we can add as much `donut-chart` tags as we want to display as many donutcharts as we want. But the problem now is that we don't want to have hardcoded data inside our directive. We can use an attribute like this: `data="[1,2]"` to say to our directive what data we want to display. First thing we need to do is get `data` array from the scope instead of having it hardcoded: `var data = scope.data`. Next, we have to tell our directive that we return a different object, I mean that our directive will be receiving a property on its scope called `data`: `scope: {data: '=' }` (if you have some doubts about using **&**, **@** or **=** check out this [stackoverflow question][so-question-scope]). Also take into account that we didn't have to declare the `data` variable inside a controller, we just use it as it is an attribute in our `<donut-chart>` tag.  

{% gist 8576512 %} 

###Adding ng-repeat

Let's add `ng-repeat` directive to draw several charts. To do that, we create an array of arrays with `ng-init` and we don't need to repeat the `<donut-chart>` directive, we just use our `ng-repeat` inside our `donut-chart`: 

{% gist 8597904 %} 

###Using a controller

We can create a controller and declare the data there. We simple move the use of `ng-init` to our controller: 

{% gist 8598094 %}

NEXT: Continue from minute 32:00



<!-- [id_ref]: URL--> 
[url-victor-powell]: http://vctr.me/ 
[angularjs-directives-doc]: http://docs.angularjs.org/guide/directive 
[announcements2014]: https://www.youtube.com/watch?v=aqHBLS_6gF8&feature=youtu.be 
[url-d3]: http://d3js.org/
[so-question-scope]: https://stackoverflow.com/questions/14908133/what-is-the-difference-between-vs-and-in-angularjs
