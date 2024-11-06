---
title: "Controlled vs Uncontrolled Components in React: Pros, Cons, and Use Cases"
seoDescription: "Explore the pros, cons, and use cases of controlled and uncontrolled components in React, including state management, performance, and ease of use"
datePublished: Wed Nov 06 2024 04:40:30 GMT+0000 (Coordinated Universal Time)
cuid: cm35e4vkd000a09l0f3kb7jtd
slug: controlled-vs-uncontrolled-components-in-react-pros-cons-and-use-cases
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730867580631/62531526-e7b1-47e5-b279-780eb18cfc94.png
tags: frontend, web-development, webdev, reactjs, typescript, frontend-development, front-end-development, controlled-and-uncontrolled-components

---

**Both controlled and uncontrolled components are concerned with how data or state is managed inside a react component**. While controlled and uncontrolled components can apply more broadly to any component that manages state, they are most commonly used in the context of form fields because form inputs are the elements that frequently need to manage and track user entered data.

But what do they even mean?

## 🖥️ Controlled Components

Controlled component use React state to manage form input values. When you use controlled components, you bind the form element to a piece of React state and use `onChange` event handler to update the state as the user types or interacts with the input.

```javascript
function App() {
    const [name, setName] = useState("");
      
    return (
        <div>
            <input 
            type="text" 
            value={name} 
            onChange={(e: React.ChangeEvent<HTMLInputElement>) => {
                setName(e.target.value)   
            }} />
        </div>
    )
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730861208601/e5874858-7aae-4327-ac15-237dd8994858.png align="center")

### ✅ Advantages

* ✨ **Real-Time Updates**: Since React tracks every change, it is easier to validate input in real time and provide instant feedback.
    
* ✨ **Improved Reusability**: Controlled components can be reused easily because they rely on state passed down through props.
    
* ✨ **Predictable state management**: The form input’s value is always tied to the component's state, making it easy to track and manage. You can predict and control the form behavior as it’s always in sync with the state.
    

### 🚨 Disadvantages

* ⚠️ **Performance**: Each change triggers a re-render since react uses `setState` to update the state. Thus, frequent re-rendering can slow down the application, especially if you have multiple controlled fields.
    
* ⚠️ **Code Overhead**: Setting up state and writing `onChange` handlers for each input can make the code longer, especially in case of larger forms.
    
* ⚠️ Increased Memory Usage: Every input value must be stored in the component’s state which can eventually increase memory usage and affect performance.
    

## ⚙️ Uncontrolled Components

Uncontrolled components don’t bind their value to React state. Instead, they rely on the DOM to handle the form state. You retrieve values only when you need them, typically by using a ref. Instead of updating React state on every keystroke, you only get the value from the DOM when necessary, such as during form submission.

You might think where in the DOM does the value reside in? Well, the values are stored in event object in the DOM and you can access it via event object.

```javascript
function App() {
    const emailRef = useRef<HTMLInputElement | null>(null);
    
    console.log(emailRef.current?.value);

    return (
        <input ref={emailRef} type="text" />
    )
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730866382022/5567499f-3989-420b-9082-646578f43aa7.png align="center")

### ✅ Advantages

* ✨ **Simpler and Less Boilerplate**: Since the form element do not require event handlers to update the form values on input change, there will be less boilerplate code as compared to controlled components.
    
* ✨ **Fewer Re-renders**: The form elements do not have to re-render to update value in the form elements. Hence, this improves form performance.
    
* ✨ **Good for Simple or Static Forms**: Uncontrolled component is a better choice for forms that don’t require much interactivity, validation or dynamic behavior.
    
* ✨ **Better Support for File Inputs**: Uncontrolled components are better suited for handling file inputs, as they allow you to access the file input’s value directly through the DOM.
    

### 🚨 Disadvantages

* ⚠️ **Lack of control**: Uncontrolled components do not maintain their own state, which means you have no direct control over the values of the form elements. This can lead to unexpected behavior and make it harder to manage form data.
    
* ⚠️ **Tight coupling to DOM**: Uncontrolled components are tightly coupled to the DOM, making it harder to change or refactor the component without affecting the underlying DOM structure.
    
* ⚠️ **No support for validation**: Uncontrolled components do not provide built-in support for form validation, making it harder to ensure data consistency and integrity.
    
* ⚠️ **Less scalable**: Uncontrolled components can become less scalable as the complexity of the form increases, making it harder to manage and maintain the component.