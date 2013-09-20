---
layout: post
title: "Looping over an array, beginning at an offset"
date: 2013-08-19 10:30
comments: true
categories: [development,javascript] 
---

I'm playing around with building a little Texas Hold 'em simulator. I set up the players as an array, and started a hand. I realized pretty quickly that I'll need to start play at a different player each hand, which means looping over the array with a different offset every time. I wondered, how do I loop over the entire array, beginning at a position other than 0? And then the magical modulus operator came to my rescue.

``` javascript
// set up the array
var fruit = ['banana','apple','orange','kiwi','watermelon'];
 
// set an offset
var offset = 3;
 
// loop over the array
for (var i = 0; i < fruit.length; i++) {
 
  // calculate the iterator
  var it = (i + offset) % fruit.length;
 
  // access the array element, using the fancy iterator
  console.log(fruit[it]);
 
}
```

```
kiwi
watermelon
banana
apple
orange
```