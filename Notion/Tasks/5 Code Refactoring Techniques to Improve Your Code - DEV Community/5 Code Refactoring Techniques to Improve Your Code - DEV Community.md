---
Topics:
  - Reading
  - Software Development
URL: https://dev.to/documatic/5-code-refactoring-techniques-to-improve-your-code-2lia
---
![[oar64uzgk12ciue7fuqp.png]]
## Introduction
Writing code is fun and we enjoyed it a lot. Until an error pops out from nowhere which takes quite a time to solve it. Sometimes, the error is not visible as the code runs as intended but in production can cause an error. There can be performance and accessibility-based errors which can cause a bad user experience. This kind of error can be reduced by refactoring the code.
Code refactoring involves making improvements to existing code without altering its external functionality. It is one of the core parts of programming, which can not be overlooked otherwise, you will not achieve a better version of your code. Code refactoring can enhance the readability, maintainability, and scalability of the code. It is also intended to increase performance and increase developer productivity.
Today, we are going to look into some techniques that can help you in refactoring your code in a better way.
Now, let’s get started.
## How to integrate Refactoring
Before looking for techniques to improve refactoring, let’s see how you can integrate code refactoring into your coding process. The below suggestion you can use for the purpose:
- You should allot time specifically for refactoring the code.
- Break down the larger refactoring problem into smaller ones for management.
- Try to involve the whole team in the refactoring process.
- Use automated tools that can help you in finding common refactoring errors.
Now, let’s start with the techniques to use for refactoring.
## Extract Method
This method involves converting code blocks into a separate method/function. This is done to improve the structure and readability of the code. It is achieved by extracting code blocks that are long, and complex into smaller and more manageable methods.
To use this technique, we first need to find a code block that performs particular tasks which are a little complex. After identification, we then extract the code and place it into a new method. Also, make sure to give a meaningful name to the method. Now, where we need the code we call them.
**Example:**
Before Refactoring
```
    function calculateInvoiceTotal(items) {
      let total = 0;
      for (let i = 0; i < items.length; i++) {
        const item = items[i];
        if (!item.quantity || !item.price) {
          console.error('Invalid item', item);
          continue;
        }
        const itemTotal = item.quantity * item.price;
        total += itemTotal;
      }
      return total;
    }
```
After Refactoring:
```
    function calculateInvoiceTotal(items) {
      let total = 0;
      for (let i = 0; i < items.length; i++) {
        const item = items[i];
        const itemTotal = calculateItemTotal(item);
        total += itemTotal;
      }
      return total;
    }
    function calculateItemTotal(item) {
      if (!item.quantity || !item.price) {
        console.error('Invalid item', item);
        return 0;
      }
      return item.quantity * item.price;
    }
```
You can see how we converted the complex code that runs inside the `for` loop into another method for simplicity and readability.
## Replace Magic Number with a Symbolic Constant
This code refactoring is for writing cleaner and more readable code. Magic numbers simply mean the hard-coded numerical value. Writing hardcoded numbers cause confusion for others as their purpose is not defined. Converting hard-coded values into a variable with a meaningful name will definitely help others to understand it. Also, you can add comments to it for further explanation. It can also help in debugging and reducing the risk of having an error in the future.
**Example:**
Before
```
    if (temperature > 32) {
        // Do something if temperature is above freezing
    }
```
After
```
    const int FREEZING_POINT = 32;
    if (temperature > FREEZING_POINT) {
        // Do something if temperature is above freezing
    }
```
## Merge Duplicated Code
duplication or identical codes can appear in the code from different places. This code does not need to be totally identical but it can perform similar tasks or extend a little bit further from the original code. Duplicated code can lead to several problems, including increased maintenance costs, difficulty in making changes to the codebase, and a higher risk of introducing bugs.
While refactoring your code, you have to look out for duplicate codes. When finding such code, one method to deal with this is converting such codes into a single reusable function/method.
Example:
Before
```
    function calculateTotal(numbers) {
      let total = 0;
      for (let i = 0; i < numbers.length; i++) {
        total += numbers[i];
      }
      return total;
    }
    function calculateAverage(numbers) {
      let total = 0;
      for (let i = 0; i < numbers.length; i++) {
        total += numbers[i];
      }
      const average = total / numbers.length;
      return average;
    }
```
After
```
    function calculateSum(numbers) {
      let total = 0;
      for (let i = 0; i < numbers.length; i++) {
        total += numbers[i];
      }
      return total;
    }
    function calculateTotal(numbers) {
      return calculateSum(numbers);
    }
    function calculateAverage(numbers) {
      const total = calculateSum(numbers);
      const average = total / numbers.length;
      return average;
    }
```
In the before code example, we were doing the sum and doing it again for finding the average. In the after, we have replaced that with the function that provides a sum to both of them.
## Simplifying Methods
It is quite similar to identifying as you are looking for methods/functions to optimize. Simplifying methods can be done for logic or to make it readable and cleaner. This technique can help you in reducing lines of code.
This method can be broken down into smaller code blocks that you can find in a function to optimize. Here are those:
- Remove unnecessary variables and expressions: There can be some variables or expressions that you leave out for debugging but forget to remove such as console.log in JavaScript. Remove those and also
- Using built-in function: Sometimes using an in-built function of the library or language will be better. As the same functionality can be achieved with less code.
- Simplify conditional statements: If a method has complex conditional statements, consider simplifying them by combining conditions or using the ternary operator. ## Using Lazy Load
It is a technique where objects are loaded only when they are needed. This can improve the performance of your application by reducing the amount of memory used. This results in faster loading of the application.
This technique is quite popular in web development. Especially with JavaScript frameworks like React where you can import different components through lazy loading. This can also be used to load images as per requirement.
**Example:**
Before
```
    import React from 'react';
    import MyComponent from './MyComponent';
    const App = () => {
      return (
        <div>
          <h1>My App</h1>
          <MyComponent />
        </div>
      );
    }
    export default App;
```
After:
```
    import React, { lazy, Suspense } from 'react';
    const MyComponent = lazy(() => import('./MyComponent'));
    const App = () => {
      return (
        <div>
          <h1>My App</h1>
          <Suspense fallback={<div>Loading...</div>}>
            <MyComponent />
          </Suspense>
        </div>
      );
    }
    export default App;
```
In the updated version, we use the `lazy` function to import the `MyComponent` component asynchronously. This means that the component is only loaded when it is actually needed, improving the overall performance of our app. We also use the `Suspense` component to display a fallback UI while the component is being loaded.
## Conclusion
Refactoring is an essential practice for any developer who wants to improve the quality, performance, and maintainability of their code. By taking the time to analyze and optimize your code, you can eliminate redundancies, reduce complexity, and create a more efficient and scalable application.
By continuously reviewing and improving your code, you can create a more robust and resilient application. I hope this article has helped you to understand some refactoring techniques. Thanks for reading the article.