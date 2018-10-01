---
layout: post
title:  "Python Learnings"
date:   2018-09-30 00:15:12 -0500
categories: jekyll update
mathjax: true
---
## Python Learnings:
After reading the following books :
1. `20-python-libraries-you-arent-using-but-should` - from O'Reilly
2. `how-to-make-mistakes-in-python` - from O'Reilly
3. a little bit of `fluent-python-2015`

I learned the following things:
- Don’t use `list` as argument inside class function definition - if you append on the list, then each class instance will have affect on that list, irrespective of it being in argument.
- use `logging` module as much as you can, helps to
- use `pylint` to check the code, it gives you error if something is a miss
- `signature` in `inspect` module lets you find the function arguments and default values
- `attrgetter` is good function to get the values
- use `namedtuples`, if you are using multiple tuples, it is easier to comprehend and doesn’t use much space
- Searching in `set` and `dict` is way faster compared to that in `list`
- `set` is good function - underline it uses hash function to store the values
- `dict.counter` is good object for counting the number of elements that are being entered in dictionary
- `defaultdict` is good if you have to do -> if you have to search if the key exist in dict every time, it saves 3 lookup when entering the new key into the dict
- `methodcaller` is good function, if you have to repeat some function
- `reduce` is also function to be used - it is used to reduce the list based on some function eg. reduce(sum, range(1,100)) -> gives sum of 1 to 99. sum -> defined in operator module, reduce-> functools
- `partial` function in `functools` - helps to partially define a function, so that it can be use later on

```python
    eg.
    from operator import mul
    from functools import partial
    mul_by_3 = partial(mul, 3)
    ans = mul_by_3(7)  
    value of ans is 21
```
- `pytutor` is good online resource to visualize how it is going in stack
- `dis` in module `dis` is also good to understand how the values are set with a variable. It is a disassembler and helps understand the various calls inside stack
- when using `set`, try not to make it from list, if possible do s = {1,2,3,4} instead of s=set([1,2,3,4]). 1st one is faster, can be seen from dis
- don’t use mutable object inside non mutable objects. eg tup = (1, 2, [4,5]) [ we have list(mutable) inside tuple(non mutable)], if we do tup[2].append([6,7])). This will append the list and also gives an error - unknown behavior
- for randomly choosing a single value inside, use `choice` inside `random` module
- if there are lot of numbers in list, better use `array` in python rather than `list`
- `bisect` function does binary search, also its `insort` helps in insertion when the list is sorted
- `@property` decorator is used to create “read-only” objects in a class .
- We can check the class variables by using class.__dict__ method
- __slots__ should be used only to save memory, and only if that is a real issue.
- operator module/library in python provides built in lambda functions for frequently used items
- keep __eq__ and __hash__ close in source code, since they need to work together
- When you create your own `ABC`(Abstract Base Class) by inheriting from available ABCs, then you have to implement all the functions required to complete the ABC class and not only what you need.
- `register` is used to define virtual subclasses for a given class. It is used as a decorator
