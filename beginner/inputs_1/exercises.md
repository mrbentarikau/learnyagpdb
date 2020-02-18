---
description: Exercises for working with parseArgs and .CmdArgs
---

# Exercises

1. **Write a CC that will send the message the user provides, with optional channel at the end** Use `parseArgs` with an optional argument \(the channel\). The channel argument should default to the current channel. If the message is sent in a channel which is not the current channel, it should send a message in the current channel confirming the message was sent. _Note that a caveat of implementing this with parseArgs is that you will have to send multiword sentences enclosed in quotes. ****_
2. **Write a CC that will send the name of the current channel or the channel provided.** Hint: Look at the docs for the `Channel` object to see what properties it has, and which one provides the name. Use `parseArgs` with optional argument. 
3. **Write a CC that when given an integer N greater than or equal to zero returns a string of the number followed by an apostrophe then the** [**ordinal suffix**](https://en.wikipedia.org/wiki/Ordinal_numeral)**. Example returns would be: 20'th 42'nd 101'st...** This CC uses functions modulo `mod` and division `div` and `joinStr` for return. For parsing the input to value for variable `$N` we use `parseArgs`. Also conditional execution statements `if-else`are being used. Here you have to think of an algorithm, how to get correct suffixes for correct numbers and then generalize it to CC code.  [Example solution](https://pastebin.com/AdSYe5k8). ****

\_\_

