---
layout: post
title:      "What is an Argument"
date:       2019-04-23 09:12:54 -0400
permalink:  what_is_an_argument
---


When you first start out on your journey to learn to code, there are many different challenges that you will face and new concepts to learn. One thing that many folks struggle with in the beginning is what an argument is and how it is used in a function. An argument is a piece of information that is given to a function, which it then uses to perform some operation. When we write a function that accepts an argument, we don't know what information that argument will contain, only that it will be of a specific type(array, string, etc.). I think this is the crux of what people struggle with when first working with arguments. I often see code like the following: 

```
function removeLastElementFromArray(array) {   //array is the argument to our function
  array = [cat, dog, hampster, fish];
  array.pop();
	return array;
}
// return value will always be [cat, dog, hampster] 
```

The problem with this code is that we are actually overwriting the information that our code is intended to operate on. In this line, `function removeLastElementFromArray(array)`, array represents the piece of information that will be given to our function when it is called. We don't know what that information will be specifically, only that it will be an array(or at least it should be, if it is not than our function has been used improperly and that is a different problem). When we include this line, `array = [cat, dog, hampster, fish];`, our argument gets redefined and we are now using hardcoded data. That means our function will always return the same thing no matter what information we give it(this is bad). With that in mind, we can change our function so that it will behave as intended.

```
function removeLastElementFromArray(array) { // array is already defined when it is passed to our function
  array.pop();
	return array;
}
// now the return value will be whatever array was given to our function with it's last element removed
```

Arguments are a big part of what makes functions so incredibly useful. They allow us to dynamically operate on data even when we don't know what that data will be. So remember, an argument represents information that will be passed to our function. It is information that we want to use or manipulate. If we redefine it, our function will not behave as expected and won't be useful to us.  

