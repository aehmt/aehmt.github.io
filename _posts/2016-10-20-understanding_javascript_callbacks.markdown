---
layout: post
title:  "understanding javascript callbacks "
date:   2016-10-20 02:06:28 +0000
---



In javascript, functions are first-class objects, and they can be used in a first class manner like any other object(arrays, strings, numbers, etc). They can be stored as variables, passed as arguments to functions.

JavaScript is asynchronous? 
This means that under some circumstances code might not be executed top to bottom. 


Callback functions are derived from a programming paradigm known as functional programming. At a fundamental level, functional programming specifies the use of functions as arguments. 

A callback function, also known as a higher-order function, is a function that is passed another function as a parameter, and the callback function is called inside the other function.

The callback function is not executed immediately. It is “called back” (hence the name) at some specified point inside the containing function’s body. So, even though the first jQuery example looked like this:

> ``` 
> $("#button").click(function() {
>   alert("Button Clicked");
> });
```

the anonymous function will be called later inside the function body. Even without a name, it can still be accessed later via the arguments object by the containing function.


As you see in the example, a function is passed as a parameter to the click method. And the click method will call (or execute) the callback function we passed to it. This example is a typical use of callback functions in JavaScript, and one widely used in jQuery.



How Callback Functions Work?

We can pass functions around like variables and return them in functions and use them in other functions. When we pass a callback function as an argument to another function, we are only passing the function definition. We are not executing the function in the parameter. In other words, we aren’t passing the function with the trailing pair of executing parenthesis () like we do when we are executing a function.

And since the containing function has the callback function in its parameter as a function definition, it can execute the callback anytime.

Note that the callback function is not executed immediately. It is “called back” (hence the name) at some specified point inside the containing function’s body. So, even though the first jQuery example looked like this:

>```
>//The anonymous function is not being executed there in the parameter. 
>//The item is a callback function
>
>$("#button").click(function() {
>  alert("Button Clicked");
});
```

the anonymous function will be called later inside the function body. Even without a name, it can still be accessed later via the arguments object by the containing function.

Callback Functions Are Closures

When we pass a callback function as an argument to another function, the callback is executed at some point inside the containing function’s body just as if the callback were defined in the containing function. This means the callback is a closure.


What is a closure?

A closure is an inner function that has access to the outer (enclosing) function’s variables—scope chain. The closure has three scope chains: it has access to its own scope (variables defined between its curly brackets), it has access to the outer function’s variables, and it has access to the global variables.


Example with/without Callback




