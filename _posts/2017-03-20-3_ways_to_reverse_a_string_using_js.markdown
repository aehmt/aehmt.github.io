---
layout: post
title:  "3 ways to reverse a string using JS"
date:   2017-03-20 02:03:20 +0000
---



1) Using string concatenation:

This implementation is probably the easiest way to implement a string reverse. We declare a for-loop with an index which start at 'string length - 1', a reversed string variable '' and a test condition to stop the loop at 'index >= 0' while decrementing the index variable. As an execution statement we concatenate the character at given index to our the reversed variable.

```
function reverse(string) {
  for (var i = string.length - 1, reversed = ''; i >= 0; i--) {
    reversed += string[i]
  }
  return reversed
}
```



2) Using two arrays:

This one might be easier to follow compared to the first implementation. Here we declare two index variable in for-loop initialization. One is the highest index number of given string and other is the lowest which starts from '0'. We start pushing last character of given string to an empty array, once iteration is over, we join the elements to create the reversed version of given string. 


```
function reverse(string) {
  var reversed = [];
  for (var i = string.length - 1, j = 0; i >= 0; i--, j++)
    reversed[j] = string[i];
  return reversed.join('');
}
```



3) Recursion:

The example below iterates through the string while excluding the first character on each iteration, which is appended to the result until substring turns into a empty string. Only then base case statement returns true and function returns the reversed string.

```
function reverse(string) {
  return (string === '') ? '' : reverse(string.substr(1)) + string.charAt(0);
}
```

