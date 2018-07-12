---
layout: post
title:      "The First Web App"
date:       2018-07-12 02:02:45 +0000
permalink:  the_first_web_app
---

I was fortunate going into writing my first web app in that I knew exactly what I wanted to do. It's an idea that I've been rolling around for several months and will be a small part of something much bigger down the road. This app is used to build and store custom recipes. I found it very satisfying be able to finally start to bring to life this idea that's been fermenting for so long, even if it is only the first small piece. 

The begining is of course the most daunting part of starting the project, especially when it's the first you've made. I found seting up the environment to be the most challenging, Thank goodness for, big surpise here, the internet, and a former Fatiron student who wrote an [excellent piece](http://blog.flatironschool.com/how-to-build-a-sinatra-web-app-in-10-steps/) on setting up a Sinatra web app. After getting the environment set up it was (mostly)smooth sailing. 

As I built my routes I noticed little things that had gone under the radar before, like creating navigation between pages rather than just building a particular functionality. While setting up my form to create a recipe I ran into the issue of how to properly associate a recipe with an igredient and the amount of that ingredient. My first instinct was to have the amount as an attribute of an ingredient but that results in a database littered with duplicate ingredients. What I really wanted was a nested table as an attribute of the recipe that could store ingredients and there amounts. A join table served the same purpose but I'm still left feeling like there could be a better way. 

Once I got that squared away the finish line was in sight. Tidy up my code a bit, add some flash messages, and put a bow on it.
