---
layout: post
title:  Inheritance Patterns in JS - Part 3
date:   2015-09-05
description: The Prototypal and Pseudoclassical Inheritance Patterns
---
In Part 2, we looked at the Functional Shared inheritance pattern, and saw how it utilized a separate object containing methods that would be shared by all instances of a class. Today, we look at more efficient ways to do this type of method sharing, using some of Javascript's built-in keywords. This is known as the Prototypal inheritance pattern.

{% highlight javascript %}
var Stack = function() {
  var newStack = Object.create(Stack.prototype);
  newStack.storage = [];
  return newStack;
};

Stack.prototype.pop = function() {
  return this.storage.pop();
};

Stack.prototype.push = function(item) {
  this.storage.push(item);
};

Stack.prototype.length = function() {
  return this.storage.length;
};

var myStack1 = Stack();
var myStack2 = Stack();
var myStack3 = Stack();
{% endhighlight %}

The key differences here lie in the use of Stack.prototype, and the Object.create method within our constructor function. Because nearly everything in Javascript is an object, and any function could potentially be a constructor for a class, Javascript objects have a built-in prototype property that can hold shared methods, just in case that object becomes a constructor. By defining our pop, push, and length methods as properties in the prototype object, we are populating this shared method space for our Stack constructor. The use of Object.create here does essentially the same thing as in our example using the Functional Shared pattern when we used a helper extend function to give our object all of the methods from our shared prototype. The one main advantage of using Object.create though is that this gives us ongoing lookup for methods in the prototype object. In the example using extend, we are creating a one-time copy of pointers to our methods - if we later add in more methods into a shared methods object after an instance has already been created, that instance would not have access to the new methods. However, using Object.create ensures us that all instances will have access to methods defined on the prototype, regardless of whether the methods were defined before or after the instance was created.

The last pattern is a slight modification on the Prototypal inheritance pattern, and is meant to more closely approximate the syntax of classes from other programming languages. This pattern is called the Pseudoclassical inheritance pattern.

{% highlight javascript %}
var Stack = function() {
  this.storage = [];
};

Stack.prototype.pop = function() {
  return this.storage.pop();
};

Stack.prototype.push = function(item) {
  this.storage.push(item);
};

Stack.prototype.length = function() {
  return this.storage.length;
};

var myStack1 = new Stack();
{% endhighlight %}

The main differences to note here are in the Stack constructor, and in the way we create instances of the Stack class using the new keyword. Some Javascript compilers have optimizations written for the use of the new keyword, but otherwise, this is functionally identical to the Prototypal pattern. The use of the new keyword essentially substitutes certain lines in the constructor to make them unnecessary. See the commented lines below for an example:

{% highlight javascript %}
var prototypalStack = function() {
  var newStack = Object.create(Stack.prototype);
  newStack.storage = [];
  return newStack;
}

var Stack = function() {
  // var this = Object.create(Stack.prototype);
  this.storage = [];
  // return this;
};

// prototype methods definitions...

var myStack1 = new Stack();
{% endhighlight %}

The commented lines in the constructor show off essentially what is happening in Javascript when we use the new keyword. The constructor knows that the keyword this will be bound to the prototype object, then we additionally specify any information that is unique to an instance of the class. Finally, we return the object referenced by this. The new keyword renders the commented lines in the Stack constructor unnecessary since it is automatically one. I've included a prototypal version of the Stack constructor to highlight the line by line similarity.
