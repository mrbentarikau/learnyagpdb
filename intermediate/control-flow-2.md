---
description: Learn how range and with actions work
---

# \[WIP\] Control Flow 2

You've got to this point so far, and you've learnt many, if not all, of the basic things you need for a YAGPDB Custom Command.

In Control Flow 2, you will learn two very useful ways which can help optimize your code - make it shorter, while doing the same thing, in addition to allowing you to do things that you couldn't have done before. How do we do this?

## The Range Action

_If you're worked with other programming languages in the past, `range` is like a `for` loop._   
  
The `range` action is defined like the following:

```go
// Iterating over values
{{ range pipeline }}
    // Action executed with each value of this pipeline
{{ else }} // Note that this is optional, you can skip to the end clause
    // Action executed when the length of the pipeline is 0
{{ end }}

// Iterating over key-value pairs
{{ range $key, $value := pipeline }} // $key and $value can be any variable
    // Here, $key is the key (for maps / sdicts/ dicts) and the index, 
    // starting from 0 for arrays / slices. $value will be the corresponding
    // value.
{{ else }} // Optional
    // Action executed when length of pipeline is 0.
{{ end }}
```

There's a lot of lingo here that might be new for you. `pipeline` is either an array / slice \(a _cslice_ or normal _slice_\)  or map \(a _dict_ or _sdict_\). We also refer to maps as key-value pairs, as that is what they are \(keys corresponding to values\). `Iterating` is a fancy word for "looping over", or doing an action for every element in the pipeline.

This really isn't the most useful example, so let's jump right in with a practical example of when you might use `range`.

### Example 1: Adding multiple roles to users with the join message

It's likely a problem many of us have dealt with when we started our servers. You might have first started to look at Autorole to help. Unfortunately, it only supports giving one role. You might have ended there, but you may also have looked to join message, and the `addRoleID` template \(or a similar template\). 

{% hint style="success" %}
**Pro Tip:** A simpler way of adding multiple roles to a user on join is using AutomoderatorV2 and an "On member joined" trigger with "give role" effect, but for educational purposes, this example will use join message.
{% endhint %}

Your final code in join message might have ended up looking a little bit like this \(where x, y, z, a, b, and c are placeholders for IDs\).

```go
{{ addRoleID x }}
{{ addRoleID y }}
{{ addRoleID z }}
{{ addRoleID a }}
{{ addRoleID b }}
{{ addRoleID c }}
// And so on
```

Some might have stopped here \(I sure did when I did this the first time\). But as you add more and more role IDs, it becomes harder and harder to maintain this code. How do we fix this?

#### Range Use Case 1: Reducing repetitive code

The first, and arguably the simplest use of `range` is to reduce repetitive code. Given a template or function which is executed multiple times \(in our case, `addRoleID` with varying arguments, we can put these arguments into an array or cslice and then loop over it with range, calling the function each iteration. We'll explain the abstract part of this later, but here's the simplified code for the above:

```go
{{ $ids := cslice x y z a b c }}
{{- range $ids }}
    {{ addRoleID . }}
{{- end }}
```

Let's go through our code step-by-step, as this may read like gibberish to you at first - _What's that . doing there? What the heck are those hyphens after {{ doing??_

1. `{{ $ids := cslice x y z a b c }}`: We construct a slice or array of role IDs. Very straightforward. 
2. `{{- range $ids }}`: Here is where the fun really starts. Let's start with the `{{-` rather than just `{{`: **Whitespace is rendered as output in a range action,** meaning that if you have newlines or indents and are ranging over a large enough set of data, you may find that you're getting a "Response exceeded 2K characters" error for apparently no reason. `{{-` before the opening range action and before the end clause solves this issue. As to why this works, it is because `{{-` or `-}}` strip whitespace in a given direction, meaning that the - before the opening range statement would strip whitespace to the right, same with the - before the closing end statement.  In this specific case, we do not necessarily need these as there 1\) is not enough data for it to hit a 2K character error and 2\) we send no text afterwards, so newlines would not be an issue. However, it's good practice and prevents some frustration when you see that strange "Response exceeded 2K characters" error without apparent cause.  Finally, the `range $ids` part declares the range statement itself. We declare it with the `range pipeline` syntax rather than `range $key, $value := pipeline` syntax as in this specific case we do not need the index of the slice we are iterating over. 
3. `{{ addRoleID . }}` What's this? We see the `addRoleID` template, but we also see this `.`. Normally, the dot refers to all the data available in CCs: for example, `.Guild`. \[TO BE CONTINUED\]

