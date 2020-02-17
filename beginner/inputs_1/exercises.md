---
description: Exercises for working with parseArgs and .CmdArgs
---

# Exercises

1. **Write a CC that will send the message the user provides, with optional channel at the end** Use `parseArgs` with an optional argument \(the channel\). The channel argument should default to the current channel. If the message is sent in a channel which is not the current channel, it should send a message in the current channel confirming the message was sent. _Note that a caveat of implementing this with parseArgs is that you will have to send multiword sentences enclosed in quotes. ****_
2. **Write a CC that will send the name of the current channel or the channel provided.** Hint: Look at the docs for the `Channel` object to see what properties it has, and which one provides the name. Use `parseArgs` with optional argument. ****

\_\_

