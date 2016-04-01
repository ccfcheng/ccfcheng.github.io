---
layout: post
title:  Inheritance Patterns in JS - Part 2
date:   2015-08-30
description: The Functional Shared Inheritance Pattern
---
In Part 1, we looked at the Functional inheritance pattern, and saw how it created duplicate methods on each instantiation of the class. Today, we look at the Functional Shared pattern. In this pattern, we decouple the constructor from its methods, as shown below:

{% highlight javascript %}
var Stack = function() {
  var newStack = {};

  newStack.storage = [];
  extend(newStack, Stack.methods);

  return newStack;
};

Stack.methods = {
  pop: function() {
    return this.storage.pop();
  },
  push: function(item) {
    this.storage.push(item);
  },
  length: function() {
    return this.storage.length;
  }
};

var extend = function(destination, origin) {
  for (var key in origin) {
    destination[key] = origin[key];
  }
};

var myStack1 = Stack();
var myStack2 = Stack();
var myStack3 = Stack();
{% endhighlight %}

The main difference here is the fact that we have extracted the Stack methods out into their own object. This makes it so that when we create our three new instances of a Stack, we do not duplicate our methods. The extend function we have written here allows us that functionality. When we create the newStack object, we also extend to it all the properties of the Stack.methods object. Thus, each new instance of a Stack will have access to the Stack methods through pointers to the functions, rather than having to create duplicate versions of the methods themselves. We keep the storage property on the constructor itself though, since each new instance needs its own unique copy of the storage array. In general, when using this inheritance pattern, we can put any properties that are unique to each instance of a class inside the class constructor, whereas any methods that are shared by all class members can be pulled out to a separate object. However, one caveat in using this pattern is the fact that we now had to use the 'this' keyword in our definitions of the stack methods in order to have access to the particular storage array owned by that specific instance of the Stack. This simply means that if we ever use our stack methods in a situation where we lose our intended binding for 'this', we need to remember to do a proper binding of 'this' to have our methods function correctly. In Part 3, we will see our third inheritance pattern, called Prototypal Inheritance!
