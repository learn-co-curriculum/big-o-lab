# Big O Shopping

## Objectives
* Practice calculating the big o by reading functions
* Practice spotting hidden loops


### Introduction

For this lab we want to use our knowledge of big O to write some functions for our online store, as well as consider the time complexity of some of our existing functions.  

> Note: From this point on, we will be using big o to calculate how long an algorithm takes to run.  If you come across other mechanisms of calculating time complexity outside of this curriculum, they are mentioning alternatives to considering the worst case scenario, which would not be big o.  However, as explained earlier, considering worst case is preferred.  

When answering the questions below, please do not rely on any Javascript methods like `find`, `include`, `filter`,  `indexOf`, or `sort`.  Methods like those operate through using loops.  And we are trying to see where these loops occur, and the cost of them, by implementing them directly.

## A first example

Let's write a function called total that will add up a list of prices.  Essentially, we would go through each item in the list and add up each corresponding element.   What is the big O of our function `totalArray()` below.  Try to come up with some code for the function as well as an explanation before looking.

Take out your Learn IDE, a piece of paper, or your favorite text editor and get to it.

Are you able to do it?


Ok, after you've given it a shot, you can look.


```javascript
let cart = [1, 5, 3, 12]
function totalArray(cart) {
  let t = 0
  for (var i = 0, l = cart.length; i < l; i++) {
      // loop 1
      t += cart[i][item]
  }

  return t
}

```

Well consider how the cost of the function changes as the size of our cart changes.  Or another way of thinking about it is how many nested loops are there that depend on the size of the array.  There is one loop that iterates through each element of the array.  So the **big O of totalArray is n**.

Now consider the big O of our `total` function below.  Our function here also adds up a list of items, however now each element in our cart array is a hash, so we have to modify our function accordingly.  What is the big O of the function `total`, if we say that a cart has length of n:

```javascript

let cart = [
  {'socks': 3},
  {'jeans': 30},
 {'shirt': 15},
 {'hat': 12},
 {'belt': 14}
]

function total() {
  let t = 0

  for (var i = 0, l = cart.length; i < l; i++) { // loop 1
    for (var item in cart[i]) { // loop 2
      t += cart[i][item]
    }
  }

  return t
}
```

Answer:

Count carefully.  This one is tricky.

![](https://s3-us-west-2.amazonaws.com/curriculum-content/web-development/algorithms/You-can-do-it-BABY.jpg)


Ok, this is tricky.  The length of the cart is n, so we will have to go through the body of our first loop n times.  Given the cart above we will have to go through the body of the code five times.  Now what is the cost of each traversal through the body of loop 1.  That is, what is the cost of each traversal loop 2?  

```javascript
let cart = [{'socks': 3},  {'jeans': 30}, {'shirt': 15},
 {'hat': 12},  {'belt': 14}]

function total() {
  let t = 0
  for (var i = 0, l = cart.length; i < l; i++) { // loop 1
    for (var item in cart[i]) { // loop 2
      t += cart[i][item]
    }
  }
  return t
}
```

Well the cost of each traversal through loop two *does not* depend on the number of elements in our cart.  Our cart length can increase to a size of a thousand and so long as we only visit one item in each javascript object, the cost of loop 2 will stay constant.  So our time complexity of the entire function is the time complexity of loop 1, which is n, multiplied by the cost of loop 2, which does not depend on n, but stays constant (represented as c) for a big O(c*n) or **big O(n)**.

Ok, don't despair if you weren't able to get that one.  There are others!

## More Questions

1. Now, let's say we have a list of credit card numbers in an array, like follows.  

```javascript
let creditCards = [11234198, 818122128,
 40924922918, 11234198, 21128218, 210219219, 21981209821]

```

Write a function using two `for` loops to determine if all of the numbers are unique.  The function you write may feel pretty inefficient.  What is the worst case scenario?  What is the big O of the function.  

> ProTip: Answering an algorithmic question relies on process as much as anything else.  First make sure you understand the question.  Then, restate the  question in your process in words.  To help you solve the question, try to relate it in the real world.  In this case if I handed you twenty cards and asked you to tell me definitively whether they were all unique, could you do it?  How would you do it?  Write that down in words.  Then translate those words into code.  

When you're done, you can look at the solution below.

![](https://s3-us-west-2.amazonaws.com/curriculum-content/web-development/algorithms/trabek.gif)

Ok, let's answer this in parts.  First, here's the code.

```javascript
function allUnique(array) {
    for (var outer = 0; outer < array.length; outer++)
    {
      for (var inner = 0; inner < array.length; inner++){
        if(outer != inner){
          if(array[outer] == array[inner]){
            return false;
          }
        }
      }
    }
  return true;
}
```
As you can see, we answer the question by first choosing the element at index 0 in the array.  Then we look at every element except for the element at the index 0 (the current index).  When we look at every other element we are asking the question, do the first element equal any of the other elements.  If they equal each other then we return false, because we know that the array does not have all unique elements.  After considering the first element, we move on to consider every following element in the array.  As you can see we must make n comparisons n times, so big o is n^2.

Can you describe the worst case scenario?  Well the worst case scenario is the case in which none of the numbers are unique.  This is because if we find that one element matches another element in the array, then we can exit the function and return false.  The array does not consist of unique elements.

2. Ok, now how does the answer change if we sort the array of credit cards.

```javascript
let creditCards = [11234198, 818122128, 40924922918, 11234198, 21128218, 210219219, 21981209821]
creditCards.sort()

```

Once again, we encourage you to consider how you tackle this problem in the real world.  Imagine you had a stack of credit cards, ordered.  How would you determine if the credit cards were unique.  

Do you have an approach in mind?  

First, write down your approach.  Consider what the big O of that approach is.  

Then just check your approach with the data we gave you.  When you feel confident, you can translate your approach into code.  Finally, test your code in the console to ensure that it is working properly.

```javascript

function uniqueSorted(array){
  for (var i = 0; i < array.length -1; i++){
    if(array[i] === array[i + 1]){
      return false
    }
  }
  return true
}
```

Well how would you do this if we gave you a stack of credit cards.  Well, knowing that they are in the right order, you could take a look at the first card and the second card, if the two did not match, you would know that the first card was unique.  Then you would go to the second card and if that did not match the third, you would no the second card was unique.  Once you got to the second to last card, you if it did not match the subsequent card you would know that the list only consisted of unique elements.  Thus, we only need to visit each element one time to solve for whether the sorted array is unique.  Therefore we write the above algorithm, which is big O(n).

3. Now what if we want to know if the sorted list of credit cards is the same as as an eight digit number we choose.  What technique would we employ?  What is the big O of a function we would write?  (You do not have to write out the function.)  

```javascript
let creditCards = [11234198, 81812212, 40924922, 
11234198, 21128218, 10219219, 81209821]

creditCards.sort()

```

We would employ **binary search**, or what we casually referred to as the phone book method.  That is, we would choose a number in the center of the array, and if the number in the center is larger than our number we would only look at the numbers to the left.  Then we would keep doing this until we ran out of numbers.  So you would continue to cut the array in half until you are at one number.  We said that the definition of log2(n) is the number of times you divide a number by 2 to get down to one.   Therefore we would implement binary search which would take us log2(n).  
