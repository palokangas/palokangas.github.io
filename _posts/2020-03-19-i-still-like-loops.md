---
layout: post
title:  I still like loops
date:   2020-03-19 14:41:41 +0200
categories: programming
tags: [listcomps, loops, style, python]
---
Python list comprehensions, or map, filter and reduce are a fantastic way to get stuff done without loops. However, since performance hasn't been an issue so far, I often find myself using them only when the task is very simple and resorting to traditional (for) loops for anything a bit more complicated since this is better for readability.

This is a simple "Counting duplicates" example problem from [Codewars](https://www.codewars.com) to illustrate. The function should count the number of letters and numbers that occur in a string more than once. For example, passing string "abba" should return 2 and ('a' and 'b' both occur twice), or "indivisibilities" should also return 2 ('i' occurs seven times and 's' twice).

Clever solution with list comprehension:
```python
def duplicate_count(s):
  return len([c for c in set(s.lower()) if s.lower().count(c)>1])
```
<br />

My solution:

```python
from collections import defaultdict

def duplicate_count(text):
  """
  Create a dictionary of letter frequencies
  and return number of letters that occur more than once
  """    
  letters = defaultdict(int)
  for chr in text.lower():
    letters[chr] += 1
  return len([v for v in letters.values() if v > 1])
```

<br />

The clever solution is 86 characters on 2 lines. My solution imports a specialized container type and is 288 characters on 10 lines. Or, if you don't count the docstring, it's 185 characters on 6 lines, but I am pretty anal about docstrings. It uses a for loop and then a list comprehension.

This might still be about lack of experience in reading code, but if I have to read this, it takes about three seconds to grasp the point of the loop, even without the docstring. But quite a bit more if I have to parse the listcomp in my head. So, for the time being, loops it is. Unless we have to start optimizing the code.

**Edit:** I had to check the runtime of the code with timeit:

```
# Clever:
timeit.timeit("duplicate_count('indivisibilities')", setup="from __main__ import duplicate_count", number=1000000)     
# 4.22650526000001

# For loop:
timeit.timeit("duplicate_count('indivisibilities')", setup="from __main__ import duplicate_count", number=1000000)   
# 4.652663404999998
```
<br />
The for loop takes about 10% more time.
