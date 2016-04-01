---
layout: post
title:  Recursion vs. Iteration
date:   2015-07-11
description: Comparing runtime on a textbook example
---

In computer programming, one often needs to have a program perform a specific kind of action over and over again. The two main ways we usually do this is through recursion and iteration. In a nutshell, we loop in recursion by having a function make a call to a new version of itself during execution, and in iteration, we loop by having a function repeat an action over and over until we hit some specified ending condition. In theory, any function that can be written recursively can also be written iteratively, and vice versa. In practice, we may make a specific choice based on whether we value runtime complexity or readability, and based on the nature of the problem itself.

Let's take a text example of calculating the nth Fibonacci number. As a reminder, the Fibonacci numbers are a sequence that start with 0 and 1, then each successive number in the sequence is the sum of the previous two sequence values. For example, the first few Fibonacci numbers are 0, 1, 1, 2, 3, 5, 8, and 13. You might remember this sequence from high school math, or maybe from the opening chapters of The Da Vinci Code. In any case, let's consider a function that given an input 'n', returns the nth Fibonacci number. We will consider 0 and 1 to be the 0th and 1st Fibonacci numbers, respectively. We will start with a recursive solution.

{% highlight javascript %}
var nthFibonacci = function(n) {
  if (n === 0 || n === 1) {
    return n;
  } else {
    return nthFibonacci(n - 1) + nthFibonacci(n - 2);
  }
};
{% endhighlight %}

An advantage of recursion in the situation is that it produced code that is very readable. The fact that Fibonacci numbers are defined in terms of smaller Fibonacci numbers makes it a natural fit for a recursive definition. However, that code readability and ease of translating the definition to a Javascript function comes at a cost of algorithmic time complexity. Notice that for each call to nthFibonacci, we return two more recursive calls to nthFibonacci. So since the number of additional function calls will double each time down the recursive chain, this algorithm has exponential time complexity. For small values of n, the runtime isn't as noticeable, but as we pass in a larger value of n, the runtime increases dramatically. At n = 40, this function took 2.7 seconds to render on my computer, which is a lifetime for a single short function. So now let's explore an iterative solution.

{% highlight javascript %}
var nthFibonacci = function(n) {
  var current = 0;
  var next = 1;
  var future = 1;
  for (var i = 0; i < n; i++) {
    current = next;
    next = future;
    future = current + next;
  }
  return current;
};
{% endhighlight %}

One could make the argument that this produces code that is slightly less readable, or at least doesn't directly translate the definition of the Fibonacci sequence. However, what it sacrifices in readability, it more than makes up for in its algorithmic time complexity, which runs in linear time because we are only iterating once through the numbers from 0 to n. Indeed, the same call with n = 40 finished in less than 0.1 seconds on my computer, a dramatic runtime improvement. Depending on the type of problem we are trying to solve, we may often have to choose between readability and time complexity. Sometimes though, we may find a clever way to bridge the two. Let's end with a clever solution I saw recently that blends the time complexity advantage of an iterative solution, that still features the code readability of a recursive solution. It uses an array to essentially memoize the result of our previously calculated Fibonacci values.

{% highlight javascript %}
var nthFibonacci = function(n) {
  var fibArray = [0, 1];
  for (var i = 2; i <= n; i++) {
    fibArray[i] = fibArray[i - 1] + fibArray[i - 2];
  }
  return fibArray[n];
};
{% endhighlight %}
