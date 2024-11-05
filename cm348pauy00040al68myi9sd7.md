---
title: "Understanding Code Splitting and Lazy Loading in React"
seoTitle: "Code Splitting & Lazy Loading in React"
seoDescription: "Improve React app performance through code splitting and lazy loading to optimize load times and resource management"
datePublished: Tue Nov 05 2024 09:20:39 GMT+0000 (Coordinated Universal Time)
cuid: cm348pauy00040al68myi9sd7
slug: understanding-code-splitting-and-lazy-loading-in-react
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730798278372/dfdbca1b-a0bd-43f2-9034-0a71dc921581.png
tags: web-performance, webdevelopment, web-development, webdev, reactjs, typescript, frontend-development, performance-optimization

---

**When we talk about performance optimization in React, code splitting and lazy loading will always be the hot topic of discussion, whether it be in an interview or practical applications**.

Code splitting and lazy loading are mostly used in conjunction, but what do they actually mean?

In this blog, we will study them individually and understand why they are used together.

### 🪓 Code Splitting

<mark>Code splitting is the process of dividing your application code into multiple bundles (or chunks)</mark>. In a React app, all JavaScript files are bundled into one large file by default, which is loaded when a user first accesses the application. This can result in large file downloads and a huge loading screen.

Instead of downloading a single large JavaScript file, code splitting helps in dissecting your code into individual bundles and only download ones that are required.

**Note**: <mark>Code splitting is primarily about how you structure your application's assets for efficient loading. It doesn’t specify </mark> *<mark>when</mark>* <mark>these assets should be loaded, only that they are separated into different files.</mark>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730784752302/df1e59ae-dcd6-4951-a169-704d989479f2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730786576419/5cea183d-a02a-4206-bbb9-cc83c9fd8ae5.png align="center")

### 💤 Lazy Loading

<mark>Lazy loading is a technique used to defer the loading of certain assets or code blocks until they are needed</mark>. Which means, these code blocks or assets are only loaded when a user requests it through certain event or when they become visible on the screen.

**Note**: <mark>Lazy loading is about when to load certain parts of your code, focusing on delaying non-essential resources until they’re required by the user. This leads to quicker initial page loads since less code is loaded initially.</mark>

### 📦 Code Splitting Without Lazy Loading

Even if you only use code splitting without lazy loading, the application will still load all chunks upfront. Here’s what would happen:

* ✨ **All the bundles will still load on the initial request:** Without specifying when to load a specific bundle, all the bundles will be loaded. This doesn’t improve performance since the total size of assets loaded initially will remain the same.
    
* ✨ **Some performance benefits in caching:** Although you may not get a performance boost in the initial load, you may still benefit when it comes to caching, since the browser can cache bundles individually. So, if you update only one chunk, users only need to download the updated chunk instead of the entire bundle.
    
* ✨ **Granular updates**: When your app is updated, the code-split bundles can be cached individually, reducing the time and bandwidth needed to update the app compared to a single large bundle.
    

### 🦥 Lazy Loading Without Code Splitting

If you apply lazy loading without code splitting, here’s what would happen:

* ✨ **Delayed loading for specific components**: Lazy loading would still delay the rendering of certain components until they’re needed, but you’d be loading those components from the same large bundle, which may not save much in terms of load time.
    
* ✨ **Little or no performance gain**: Lazy loading only works if your components are in separate chunks. Without code splitting, there’s no separate bundle to load later, so lazy loading won’t provide the intended performance benefits.
    
* ✨ **Unused lazy loading**: Without code splitting, lazy loading is essentially “inactive” because all code is loaded at once. This would result in the entire application being ready immediately, so there would be no delayed loading.
    

### ⚛️ `React.Lazy()`, `<Suspense>` and `import()`

We can use `import()` for code splitting, `React.Lazy()` for lazy loading and `Suspense` to handle fallback UI while the component is loading. Here’s how each of these works:

* `import()` : It is part of JavaScript ES Module specification(ES6) and allows you to load JavaScript modules asynchronously. When you call `import(‘/path’)`, it returns a promise that resolves with the module and can be accessed using `.then` or `await`.
    

```javascript


export function add(a,b) {
    return a + b;   
}

export default function Home() {
    return (
        <h1>Hello World</h1>;
    )
}

const HomeComponent = import("./Home.jsx").then((module) => module.default));
const addFunction = import("./add.js").then((module) => {return {default: module.add})
```

* `React.Lazy()` : It is a special function provided by React to lazily load a component. It takes a function as its parameter - <mark>a function that must return a promise that resolves to a module with a default export</mark>. Thus, `import()` is the perfect match for it’s argument. When it calls the `import()` function, it receives a special object which looks like this.
    
    ```javascript
    const HomeComponent = React.Lazy(() => import("./Home"));
    console.log(HomeComponent);
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730795124683/0f4b1b57-b53c-4ef7-bc33-da5d90634865.png align="center")
    
    When you call `HomeComponent`, React monitors the status of the promise inside `_payload._result`. If the promise is pending, React knows that the component is not ready and cannot render it yet.
    
* `Suspense`: This is where suspense comes. When you wrap a lazy-loaded component inside a `Suspense`, it acts as a boundary to handle asynchronous loading states and determines whether it should display the fallback UI or the component based on the result of `_payload._result`. When React finds a lazy component within a `Suspense` boundary and the lazy component’s promise is still pending, it "suspends" rendering of that component. React then looks up the nearest `Suspense` boundary and uses its `fallback` UI as a temporary replacement for the lazy component.
    

### 🛠️ Implementing Code Splitting and Lazy Loading

Getting things all together:

### 🚀 Performance Optimization

* 📈 **Reduced Initial Load Time**: By splitting code and using lazy loading, you can significantly reduce the size of the initial JavaScript bundle that the browser must download, resulting in faster load times and improved user experience.
    
* 📈 **Optimized Resource Management**: Only the necessary resources are loaded when needed, which can lead to more efficient use of bandwidth and better performance, especially on mobile devices or slower networks.
    
* 📈 **Improved Caching**: With code splitting, different parts of the application can be cached independently. If one part changes, only that part needs to be re-downloaded, while the rest remains cached, leading to faster load times on subsequent visits.
    
* 📈 **Enhanced User Experience**: Users can interact with the app more quickly as critical parts of the application load first, while non-essential components load in the background. This leads to a smoother experience.
    
* 📈 **Scalability**: As applications grow in size and complexity, maintaining performance can become challenging. Code splitting and lazy loading help manage this complexity by ensuring that only the necessary code is executed at any given time.
    

### ⚠️ Common Mistakes and Pitfalls

* 🛑 **Neglecting** `Suspense`: Failing to wrap lazy-loaded components in a `Suspense` component can lead to rendering issues, such as the app displaying an error or crashing when the component is not yet available. Always wrap your lazy loaded component with `Suspense`.
    
* 🛑 **Over-Splitting:** Splitting too many small components can lead to a large number of requests, which can adversely affect performance due to the overhead of establishing multiple connections.
    
* 🛑 **Not Handling Errors Properly**: When using `React.lazy`, you may encounter errors during the loading of components (e.g., network issues). Not handling these errors can lead to poor user experience.
    
* 🛑 **Ignoring Browser Compatibility**: Some older browsers may not fully support dynamic imports or other features used in code splitting and lazy loading.