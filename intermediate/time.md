---
description: Learn about the time and duration datatypes
---

# Time

Working with time and duration is a major pain point for many, even those who have experience making custom commands. This does not need to be so - this chapter goes in-depth with time and duration. By the end of it, you'll be a master of time!

## The Time Type

Let's start with the _time.Time_ datatype. Think of this as a point in time. To get the current time in type _time.Time,_ we would use the `currentTime` function. It takes no arguments and returns the current time.

Let's copy and paste the following code in a new custom command:

```go
{{ currentTime }}
```

If you run this custom command, you'll see the following sent into the chat:

![Output of our time custom command](../.gitbook/assets/image%20%282%29.png)

{% hint style="info" %}
As we can see, the output of `currentTime` is in the UTC timezone by default. We'll discuss how to change this later in the chapter.
{% endhint %}

This is useful for debugging, as the details of the time are easily seen, but what if we want to format the time?

### Formatting time

YAGPDB templates provide two useful ways of formatting time: the `formatTime` function and the `Format` method on a time object.

The syntax of these two functions are as follows:

* `formatTime <time>` \(formats using RFC822 layout\)
* `formatTime <time> <layout>` \(formats using the given layout\)
* `<time>.Format <layout>` \(formats using the given layout, same as above\)

{% hint style="info" %}
The `formatTime` function simply calls the `Format` method on a time object. There is no functional difference between the two, other than that `formatTime (time)` without a format defaults to formatting using the RFC822 layout.
{% endhint %}

Now that we know what the syntax of the two functions look like, we need to explain what the _layout_ is. The layout is a template for how the time will be formatted, and you can think of it as working by replacing certain placeholders with their actual values. For example, Note that this is _not_ how it works under the hood, but it would produce an similar output.

### Time constants

