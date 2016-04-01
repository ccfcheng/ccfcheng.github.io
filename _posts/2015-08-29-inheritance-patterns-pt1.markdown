---
layout: post
title:  Inheritance Patterns in JS - Part 1
date:   2015-08-29
description: The Functional Inheritance Pattern
---

In Javascript, we have the flexibility of creating classes in a number of different ways, each with their own advantages and disadvantages. 4 main archetypes of these inheritance patterns are functional, functional-shared, prototypal, and pseudoclassical. For the sake of consistency, let's assume that we are creating a class for a stack data structure. This data structure will have methods for push, pop, and length. Let's take a look at how this might be created using functional pattern:

{% highlight javascript %}
var Stack = function() {
  var newStack = {};

  newStack.storage = [];
  newStack.pop = function() {
    return newStack.storage.pop();
  };
  newStack.push = function(item) {
    newStack.storage.push(item);
  };
  newStack.length = function() {
    return newStack.storage.length;
  };

  return newStack;
};

var myStack1 = Stack();
var myStack2 = Stack();
var myStack3 = Stack();
{% endhighlight %}

In this pattern, when we create the 3 new instances of the Stack class, we have created three unique objects, each of which have their own encapsulated pop, push, and length methods. An advantage of this inheritance pattern is the fact that we are not utilizing the 'this' keyword anywhere in our definition of the class. This means that if we ever need to use this class in any situation where the reference to 'this' is lost, we won't need to worry about binding 'this' properly in order to not break our class. However, there are some very real disadvantages as well. Because we are creating new copies of the entire newStack object on each instantiation, if we created 100 new instances of a Stack class, we would also have 100 copies of pop, push, and length methods, all of which do the same thing. This seems unnecessarily wasteful, as we should just be able to reuse these functions without creating new copies of them. Luckily, we can amend our class definition to use a different pattern called Functional Shared, in order to solve this redundancy. Check it out in Part 2!
