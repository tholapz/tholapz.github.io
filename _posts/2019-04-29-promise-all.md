---
published: true
layout: post
tags: [JavaScript, Promise.all, Programming, Web Development, Asynchronous, Error Handling, Coding, JavaScript Promises, Parallel Execution, Best Practices]
---

Learn how to efficiently execute multiple promises in parallel with JavaScript’s Promise.all, and explore strategies for effective error handling.
<!--more-->

## Modifying the behavior of Promise.all()

By now, as a JavaScript developer, you have probably used the Promise API at some point in your project. Promise offers a good alternative to using callbacks when dealing with asynchronous operations. Now, if you have a collection of promises you want all to be resolved and to aggregate the results in one promise, there’s Promise.all() and it works like this:

https://gist.github.com/6d2a565c5f3175b27256cc7ff4cb7caa

Promise.all(), by design, has a fail-fast behavior. It means it will reject as soon as one of the element has rejected.

https://gist.github.com/7fde95417db4c4d0dc2e397aae95c440

But what if you expect some promises to reject and want to get as many resolved results as possible? One way to do it is to wrap your promises around another promise that will never reject.

https://gist.github.com/3af781d3f3e86ad9c557cace9f47bb9d

The problem with this approach is that it’s ugly (citation needed). And now you lose out on the mechanism to respond to errors (since you silently ignore all of the rejected). Doing this will potentially result in a holed collection where you have to filter out failed promises.

A better alternative is to come up with your own promise aggregator function. There are many possible approaches and here’s the most basic one I’d like to propose:

https://gist.github.com/ef14f448ca341f166bf9115ca00da9cd

Let’s dissect this a bit. First, I want a way to iterate over the given array of promises. This can be done in different ways. For example, I can use Array.prototype.forEach(), conventional for…loop, or I can try the Iteration protocol.

https://gist.github.com/12bb3c35b3a746626a990e0a9483edad

Iteration protocol is implemented array prototype. Once you have retrieved the iterator, you get:

iterator.next() which returns the next element in the array and is used to iterate over each element
iterator.done to check if you have gone through every element yet
iterator.value to access the value of the element returned from calling next()
You can look up more info regarding the Iteration protocol from MDN’s documentation here (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

Next, I need a way to keep track of each promises resolving progress. I do this using a simple counter “resolving”.

https://gist.github.com/8bc720144d361ba253683025fc581e2b

The gist of the whole thing is, resolve each promises while keeping track of the progress. Once all promises are resolved, the aggregated results are ready. So resolve it as an array.

With this approach, you can control the behavior of how it should handle different cases. You can have it return null when the promise reject(), or you can simply return as many successfully resolved elements as possible, or you can have it reject as soon as any of the promises reject(), just like the default behavior of Promise.all() too!

credit to Sindre Sorhus’s p-map module (github.com/sindresorhus/p-map) that get me started.
