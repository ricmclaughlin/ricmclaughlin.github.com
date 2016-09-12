---
layout: post
title: "functional programming the hard way"
description: ""
category:
tags: []
---
{% include JB/setup %}


Write a function named map that takes two arguments. arr (array) callback (function)

Return a new array where each element of arr is transformed by the callback function and the result is pushed into the new array.

For example:

var rounded = map([0.01, 2, 9.89, Math.PI], function(num) {
  return Math.round(num);
});

console.log(rounded); // [0, 2, 10, 3]

---------------------------------------
Write a function named every that takes two arguments. arr (array) predicate (function)

Return true if the predicate function returns truthy for all elements of arr, otherwise return false. The predicate function is invoked with one argument.

element (anything)

For example:

var result = every([1, 2, 3], function(element) {
  return element % 2 === 0;
});


console.log(result); // false

result = every([2, 4, 6], function(element) {
  return element % 2 === 0;
});

console.log(result); // true
---------------------------------------

Write a function named reduce that takes three arguments. arr (array) callback (function) accum (initial accumulation value)

Reduce arr to a single value by invoking the callback function with the accum value and each element in arr. The return value of each invocation of the callback will become the new accum value. The callback function is invoked with two arguments.

accum (anything) element (anything)

For example:

var result = reduce([1, 2, 3], function(accum, element) {
  return accum + element;
}, 0);

console.log(result); // 6

------------------------------

Write a function named filter that takes two arguments. arr (array) predicate (function)

Iterate over the elements of arr and return a new array of all elements the predicate function returns truthy for. The predicate function is invoked with one argument.

element (anything)

For example:

var users = [
  { name: 'Katie', points: 240 },
  { name: 'Ralph', points: 130 }
];

var winners = filter(users, function(element) {
  return element.points >= 200;
});

console.log(winners); // [{ name: 'Katie', points: 240 }]
