---
description: >-
  In this page, you'll learn to retrieve user input using one of two methods:
  .CmdArgs and parseArgs.
---

# Inputs 1

When you starting making custom commands, it's likely that you'll be starting off with basic "tag-like" commands which respond with constant \(unchanging\) output when you run them. However, as you delve deeper into custom commands, it's likely that you'll have to work with the arguments that users provide.

YAGPDB's templates provide several ways of working with arguments, the two most popular of which are:

* the `parseArgs` "function"
* `.CmdArgs`

We'll be going through both of these in order. Note that in addition to these, there are also several other ways of working with arguments which include using `.StrippedMsg`, `.Args`, and to a certain degree `.Cmd` \(which does not concern arguments, but the trigger used\). However, we'll be talking about `.CmdArgs` and `parseArgs` today, with the others coming in a later chapter.

## parseArgs

For beginners going into intermediate level custom commands, it's likely that you'll want to start off with the `parseArgs` function. It works like any other function, with the syntax `parseArgs <minimum number of args> <error message> ...cargs`.

### parseArgs Syntax

This may seem quite confusing at first, so we'll go over each of the required arguments for parseArgs.

1. `<number of args>` - This is the minumum number of args you want parseArgs to parse or expect. For example, if you wanted to parse a user argument and a string argument, this would be `2`. If you wanted to parse a user argument which is _optional_, this would be `0`.
2. `<error message>` - This is the message that YAGPDB will send if the command was used with incorrect arguments. You can leave this blank for YAGPDB to automatically construct an error message from the descriptions of the `carg`s provided \(we'll talk about this later\).
3. `...cargs` - parseArgs is a variadic function of sorts, which means it can allow as many or as little `carg`s as you wish. `carg` is a template itself, describing the argument you want to parse. It takes the syntax `carg <type> <description>`, where type is one of the following types \(enclosed in quotes, like `"int"`\):
   * `int` \(whole number\)
   * `string` \(text\)
   * `user` \(user mentions, resolves to the user structure\)
   * `userid` \(mentions or user IDs, resolves to the ID itself\)
   * `channel` \(channel mention or ID, resolves to the channel structure\)
   * `duration` \(duration that is human-readable, i.e `10h5m` or `10 hour 5 minutes` would both resolve to the same duration\)

     The `<description>` of the `carg` is what we were talking about earlier: If a user does not execute a command with the arguments expect, YAGPDB with construct an error message from this description.

### Examples of parseArgs usage

Here are some examples of possible usage of `parseArgs`, in case you are still confused:

* `parseArgs 2 "Syntax is  " (carg "channel" "channel to send to") (carg "string" "text to send")` _This was taken from the_ [_documentation_](https://docs.yagpdb.xyz/reference/custom-command-examples#parseargs-example)
* `parseArgs 1 "" (carg "user" "user to see")`

Alright then, now you understand the format of `parseArgs`. However, it's really quite useless in it's current state, as we don't know how to retrieve the parsed arguments yet!

### Retrieving arguments from `parseArgs`

Alright, let's take this example here: `parseArgs 1 "" (carg "user" "user to see")`.

If you just copy and paste this into a new command, you can see that the output is somewhat strange - either it gives the error message based off the description of the `user` carg, or it gives us some strange-looking output, like `{[0xc036a2fd60] [0xc02c7e7b00]}`.

You may be a little confused about how you would retrieve the parsed arguments from this parseArgs, but don't worry! It's really more simple than it looks.

Take this example code: `{{ $args := parseArgs 1 "" (carg "user" "user to see") }}` and paste this into a new command. Voila! It either gives an error message or does nothing at all. Wait... what?

This is because `parseArgs` returns either an error message \(if the arguments provided were invalid\) or an object of the type _\*customcommands.ParsedArgs_. You can retrieve arguments by calling `.Get` on the reference to the parseArgs output, like the below:

```go
{{ $args := parseArgs 1 "" (carg "user" "user to see") }} {{/* We store the output of parseArgs in a variable */}}
{{ ($args.Get 0).Mention }} {{/* We .Get() the first argument, which is a user object, and then we call the .Mention property which mentions the user */}}
```

This would mention the user which you provided.

{% hint style="info" %}
In YAGPDB and coding in general, indexes start from 0, not 1, so if you wanted to get the first argument from parseArgs, you'd use `($args.Get 0)`, not `($args.Get 1)`. The latter would actually get the _second_ argument from parseArgs, rather than the first.
{% endhint %}

An example of a _complete_ command using parseArgs is the following \(very simple\) command which sends a message to a given channel. _This was taken from the_ [_documentation_](https://docs.yagpdb.xyz/reference/custom-command-examples#parseargs-example)\_\_

```go
{{ $args := parseArgs 2 "Syntax is <channel> <text>"
    (carg "channel" "channel to send to")
    (carg "string" "text to send") }}

{{ sendMessage ($args.Get 0).ID ($args.Get 1) }}
```

### IsSet

`parseArgs` has a slightly "hidden" or unknown feature called `IsSet`. When you have a number of optional arguments in your `parseArgs`, you can use `IsSet` to check whether these arguments exist. This is a little difficult to explain, so let's take the example here:

```go
{{ $args := parseArgs 0 "" (carg "user" "target user") }}
{{ if $args.IsSet 0 }}
    {{ ($args.Get 0).Mention }}, hello world!
{{ else }}
    {{ .User.Mention }}, hello world!
{{ end }}
```

Let's explain this line by line.

1. `{{ $args := parseArgs 0 "" (carg "user" "target user") }}` - Here, we put the output of parseArgs into a variable. Note the minimum number of arguments here is `0`, meaning that the user is optional. We'll see why in the next line.
2. `{{ if $args.IsSet 0 }}` - In this line, we check whether the first argument exists. If so, we send `{{ ($args.Get 0).Mention }}, hello world!`
3.  `{{ else }} ... {{ end }}`This block is executed when the optional user argument is not there. We default to mentioning the triggering user.

![](../.gitbook/assets/image-2.png)

 IsSet is extremely useful if you have several optional arguments and you want to check whether they exist / were provided by the user quickly.

### Activities

Here are some ideas for what you can do with `parseArgs`:

* Create a simple echo command which "echoes" what you said back to you
* Create a command which gets the ID of a mentioned user

## `.CmdArgs`

Sometimes, you will want to use `.CmdArgs` instead of `parseArgs`. `.CmdArgs` is much less complicated than `parseArgs` - it's simply an array of all the arguments the user provided just like YAGPDB sees them. For example, take this basic `seeargs` command and paste in the following code:

```go
{{ $allArgs := .CmdArgs }}
{{ sendMessage nil (joinStr ", " $allArgs) }}
```

Let's say we used `-seeargs "hello world"`. As YAGPDB sees text enclosed in quotations as one argument, it would repeat back to us `hello world!`. However, let's say we used `-seeargs hello world` instead. Instead of giving us the same output, it would instead repeat `hello, world`.

There's nothing too crazy about `.CmdArgs` - it's simply a slice or array of string arguments that are unparsed. If you want to parse the arguments, you will have to do it manually. Say you wanted to parse a user out of an argument, you'd have to do it with `userArg` \(which we will talk about in a later chapter\).

### Working with `.CmdArgs`

As `.CmdArgs` is an array, to get a given argument, use `index` with the index of the CmdArg you want. Note that as we said before, indexes start at zero, so if you wanted to get the first argument, you would do `(index .CmdArgs 0)`.

{% hint style="info" %}
If you wanted to get all the arguments from a given index and beyond, you could use `(slice .CmdArgs <index>)` - for example, `joinStr " " (slice .CmdArgs 1)` would give you `world YAGPDB` if you ran it with `-command hello world YAGPDB`.
{% endhint %}

### Index out of range

Inevitably, you will find yourself looking at this error when working with `.CmdArgs`. Say you have `index .CmdArgs 0`, and you run it with no arguments - YAGPDB will respond with an error saying `index out of range: 0`. How do we fix this?

The first method is using an `if` statement with just `.CmdArgs`:

```go
{{ if .CmdArgs }}
    {{ sendMessage nil (index .CmdArgs 0) }}
{{ else }}
    You provided no arguments.
{{ end }}
```

Now, this will not error. But say we use `index .CmdArgs 1` with the above? It will still error if only 1 argument is provided, because `if .CmdArgs` just checked whether there are any arguments.

We can fix this by using a comparison operator, like `eq` or `ge` and comparing `(len .CmdArgs)`:

```go
{{ if eq (len .CmdArgs) 2 }}
    {{ sendMessage nil (index .CmdArgs 1) }}
{{ else }}
    You need to provide at least 2 arguments.
{{ end }}
```

Voila! No more errors. You can do this with any comparison operator you want.

Hopefully this chapter helped you understand retrieving and parsing user input a little better. Good luck!
