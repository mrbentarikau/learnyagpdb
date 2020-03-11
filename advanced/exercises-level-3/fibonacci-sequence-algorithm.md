---
description: >-
  "The Fibonacci Sequence turns out to be the key to understanding how nature
  designs... and makes the Universe sing." // Guy Murchie
---

# Fibonacci Sequence Algorithm

The Fibonacci Sequence is the series of numbers: `0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...` So it's a sequence where the next term is the sum of the previous two terms. The first two N terms of the Fibonacci sequence are N\(0\) = 0 followed by N\(1\) = 1.

This exercise is about formulating an algorithm \(there are many\) and making a CC for Fibonacci sequence, displaying the first `N` terms of Fibonacci series. Argument for`N` is given by user. You also have to consider how to use the value user gives, considering max `N` can be 93 steps due to size of _int64_ \([why1?](https://www.wolframalpha.com/input/?i=2%5E63-1) & [why2?](https://www.wolframalpha.com/input/?i=Fibonacci%5B93%5D)\) - so what number should be inclusive.

Fibonacci sequence is defined by the linear recurrence equation.

$$
F_n=F_{n-1}+F_{n-2}
$$

But recursion as programming algorithm is not the best solution here because of time complexity which is exponential. Wherever we see a recursive solution that has repeated calls for same inputs, we can optimize it using [_Dynamic Programming_](https://weibeld.net/algorithms/recursion.html).

Using variable switching method &gt; Example solution 1.  
Using a `cslice` method &gt; Example solution 2.

