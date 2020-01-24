# Simple Counting Algorithm

Your task is to formulate an algorithm and make a custom command that works with numbers. Let's take one parameter `N`, it has to be non-negative integer and user given. Make a variable called x and set it equal to `N+3`. Now count from 0 to `N` \(both inclusive\), and for each iteration \(let's call it `i`\) that you pass - return the value of `x*(-i)`. After that update `x` to be equal to `i*N+x`. When counting finishes, return the value of `x`.

If you do these steps for N=3, you should come up with the sequence of numbers `0 -5 -14 11`.  
  
[Answer](https://pastebin.com/vNfc73zG)

