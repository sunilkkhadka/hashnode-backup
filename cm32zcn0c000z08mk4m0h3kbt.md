---
title: "Throttle Explained: Optimizing Performance by Controlling Function Executions"
seoTitle: "Throttle Control: Optimize Function Performance"
seoDescription: "Throttling controls function execution frequency, optimizing web app performance with examples and use cases for smooth operation"
datePublished: Mon Nov 04 2024 12:11:05 GMT+0000 (Coordinated Universal Time)
cuid: cm32zcn0c000z08mk4m0h3kbt
slug: throttle-explained-optimizing-performance-by-controlling-function-executions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730701600411/46ca6200-29aa-404b-9b09-e21f8011c43c.png
tags: javascript, performance, web-development, web, webdev, frontend-development, performance-optimization, throttling

---

Throttling is a technique used to rate the limit at which a function executes. <mark>The philosophy of throttling is to execute a function at most once within a specified interval of time</mark>. Meaning, it will restrict a function to execute more than once within a specified interval of time.

Let’s say we want to run a function when user types something. We are basically listening to an input event but look at the frequency at which a function may execute with and without throttle in a second.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730701131864/5df86366-4577-4c28-8444-161c2e86a1a1.png align="center")

### How to Throttle a Function

* Create a function that needs to be throttled. This function should accept a callback and a delay.
    
* Create a `flag` variable inside that function which will turn on and off every set interval of time. Initialize `let flag = false;`
    
* This function should return a new function which closes over the `flag` variable.
    
* Create a `setTimeout` that should execute the callback within a certain delay.
    
* Check if the `flag` (wait) variable is `false`. If so, turn it to `true` and run the `setTimout`. After the specified delay, turn the `flag` variable to `false`. This should allow the callback function to be executed again once the delay has been completed.
    

Here’s a simple code example:

<mark>index.html</mark>

```xml
<input type="text" placeholder="start typing..." id="input" />
<p>Output: <span id="output"></span></p>
```

<mark>script.js</mark>

```javascript
const input = document.getElementById("input");
const output = document.getElementById("output");

function throttle(cb, delay = 1000) {
    let wait = false;
    return  (text) => {
        if(!wait) {
            wait = true;
            setTimeout(() => {
                cb(text);
                wait = false;
            }, delay)
        }
    }
}

const throttleEventHandler = throttle((text) => {
    output.innerText = text;
})

input.addEventListener("input", (e) => {
    throttleEventHandler(e.target.value);
})
```

### Use Cases

* **Scroll-based animations**: Throttle the animation function to ensure it’s only called once every 100ms, regardless of how fast the user scrolls, to maintain a smooth and responsive experience.
    
* **Real Time Updates:** Throttle updates to prevent overwhelming the server with too many requests in a short period. For example, a live chat app might throttle its update requests to ensure it doesn’t send more than one request per second.
    
* **API request limiting**: Throttle API requests to prevent overwhelming the server with too many requests in a short period. For example, a weather app might throttle its API calls to ensure it doesn’t send more than one request per second.
    
* **Image loading**: Throttle image loading to prevent overwhelming the browser with too many concurrent requests. This ensures a smoother user experience and reduces the risk of browser crashes.
    
* **Use Activity Tracking**: Tracking every event in real-time can quickly overload both the client and server with data. Throttling events like mouse movement or typing to every 200ms, for instance, reduces data volume while still providing a meaningful activity history.