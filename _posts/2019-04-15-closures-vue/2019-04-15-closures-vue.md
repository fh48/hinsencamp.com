---
title: "Discover the Power of Closures in VueJS"
description: "Learn how plane JavaScript closures work and their power in VueJS! This tutorial comes with a coding challenge. 💪 Build an Emoji Picker!"
author: "Fabian Hinsenkamp"
published_at: 2019-04-15 15:06:30.486476
categories: [vue, challenge]
---

Today's frontend technology landscape, required from engineers to know about a wide variety of technologies including frameworks, libraries and packages.

No wonder, that vanilla JavaScript skills and in-depth knowledge might start spreading thin. No matter if you are just learning JavaScript, refreshing your basic knowledge or preparing for job interviews → This tutorial is for you!

Here you learn how powerful plane JavaScript closures are. Be aware, this tutorial comes with a challenge. 💪 It's all about building an elegant Emoji Picker in VueJS and leverage closures by using higher order functions.

**Let's dive right into it!**

## Function Scope

Even though closures are one of the most powerful concepts in JavaScript it is easily overlooked by many.

Nevertheless, knowing about closures is fundamental as they define which variables a function has access to during its execution. More precisely, closures define which scopes a function has access starting from its own, through all parent scopes up to the global scope.

To really master closures, it's essential to have a solid understanding of scopes first. Probably, you have also already experienced the impact of scopes yourself.
Every time you execute a function a scope is created. When ever you create variables within that function these are only accessible from within the function itself.

At the time a function is completed (by reaching a `return` statement or `}` ) all these variables are destroyed. Next time you execute the function, the same procedure is applied.

Let's look at the following example to illustrate the concept.

```javascript
function square(x) {
  const squareX = x * x;
  console.log(squareX); // 25
  return squareX;
}

const squareA = square(5);

console.log(squareA); // 25
console.log(squareX); // ReferenceError: squareX is not defined.
```

Think about scopes as the temporary context only the code within that function has access to.

While scopes have a very limited lifetime, limited by the time a function execution need to execute, in contrast a function's closure is already created when a function is initially defined. It also will remain after the execution has been completed.

## Closures

As mentioned before, closures are responsible for defining which variables are accessible the scope of a function execution. It's essential to understand, that closures do not provide copies of available variables but references to them. If you are not familiar with JavaScript's references check out this [article](https://medium.com/r/?url=https%3A%2F%2Fcodeburst.io%2Fexplaining-value-vs-reference-in-javascript-647a975e12a0).

```javascript
let globalString = "A";

function hello() {
  const localString = "C";
  console.log(globalString, localString);
}
hello(); // A C

globalString = "B";

hello(); // B C
```

The example looks probably very familiar, not any special, this explains why we barely realise how powerful closures can as it's very common to only define functions in the global scope.

However, when functions are defined within another function's scope, closures enable powerful techniques and patterns. In an object-oriented architecture, closures offer a simple but efficient way to establish **data privacy**. In more functional architectures closures are essential to **higher order functions** and **partial application** and **currying**, two more advanced programming techniques. Read more about these techniques here.

## Higher Order Functions

A functions that operate on other functions, either by taking them as arguments or by returning them, are called **higher-order functions**.

```javascript
function createMultiplier(multiplier) {
  return function(value) {
    return value * multiplier;
  };
}

const multiplyBy2 = createMultiplier(2);

console.log(multiplyBy2(5)); // 10
```

Here we finally can experience the power of having understood closures. Even though `createMultiplier` has been already successfully completed. We can still access its initial `multiplier` property.

This is possible as closures keep the reference of variables, these can even span over multiple scopes even and do not get destroyed when the context terminates. That way, we can still access a specific instance of a local variable.

## Data Privacy

```javascript
function privateVariables() {
  let privateValue = 100;
  return {
    get: function() {
      return privateValue;
    }
  };
}
console.log(privateVariables.get()); //100
```

How come closures are enabling us to implemented data privacy?

We can simply enclose variables and only allow functions within the containing (outer) function scope to access them.

You can't get at the data from an outside scope except through the object's privileged methods. This pattern also allows us to program to an interface and not the implementation itself.

## Coding Challenge: Emoji Tone Picker

![emoji picker](emoji-picker.png)

Great, that's all the theory we need for actually building something powerful in VueJS and leverage the power of closures!

In fact, higher order functions are the most interesting use case, as we already have a data privacy concept in VueJS.

Basically, are components already offering an interface through properties and the data object which isn't accessible from outside.

Here is what we want to build!

[CodeSandBox](https://codesandbox.io/embed/842rp5j4n8)

It is a component to that allows the user to choose the skin tone based an a selection of all available tones, similar to the user experience known from texting on a smart phone.

Technically, you should try to create a component that receives a single emoji as props and used higher order functions to create multiple click event handlers to select different tones.

### Hint

Emojis can be stored as HTML hex codes in string values. The folded hands emoji is &#x1F64F. To change the tone attach a colour code to it. You find the codes here.

> &#x1F64F + &#x1F3FD = 🙏🏽

## Building Challenge Extension

You want to take it one step further, and really see if you mastered closures? 
Then pass multiple emojis and make it work so you can change the skin tone of multiple emojis one at a time. 💯

## Conclusion

Closures are the reason why we can access variables of parent scopes while the related functions might have already terminated.

We can use this behaviour of JavaScript in VueJS to dynamically build methods based on dynamic arguments without polluting our components with a vast variety of variables and methods.

Thanks for reading 🙌
