---
description: Advanced sending and editing messages.
---

# Outputs 2

## Sending Messages

Up until now we have seen how to use the default **response** to send an output. However it is sometimes a bit inconvenient to use the response for producing an output. Also the output is always sent in the same channel in which the custom command is running. To solve these issues and offer more flexibility in terms of producing outputs there are specific output templates.

### `sendMessage` Template

This is the most basic and simple form of an output template. The general syntax for `sendMessage` template is :

```text
{{sendMessage channel_id message_to_be_sent}}
```

The `channel_id` is the id of the channel in which the message is to be sent. For sending the message in the same channel as the one in which the custom command is running, `channel_id` can be set to `nil`.

`message_to_be_sent` denotes the output that is to be sent as a message.In this chapter we will deal with only simple textual output \(strings\).   
  
It is important to note that `sendMessage` template by itself is an **action type** template , that is it does not produce a meaningful/usable output by itself. It simply directs the bot to send the content that is passed to it in a **separate message** from the custom command's default **response** message.

#### Example 1 :

`{{sendMessage nil "Hello World"}}`  
  
**Output:**

![](../.gitbook/assets/image%20%2818%29.png)

**Explanation :**  
As discussed earlier, the `sendMessage` template here simply sends the string as output message in the same channel as the one in which the custom command is running.

#### Example 2 :

`This is the response message{{sendMessage nil "This is a separate message"}}!`

**Output :**  


![](../.gitbook/assets/image%20%288%29.png)

**Explanation :**  
  
As discussed earlier, the `sendMessage` template sends a **separate message** as output which is different from the custom command's default response message. The `sendMessage` template being an action type template producing no meaningful output is also shown in the above example. That is the reason why the `!` appears directly after `message` in the response message although if you look at the original code the `sendMessage` template was placed between them.

{% hint style="info" %}
In the previous example it is important to note that the `sendMessage` template's output message is sent before the custom command's response message. The reason for this is that the response is generated only after the execution of the custom command is finished however the `sendMessage` template directs the bot to send a message during its execution.  
Example:   
`third{{sendMessage nil "first"}}{{sendMessage nil "second"}}`  
Output:  
 ![](../.gitbook/assets/image%20%2817%29.png)
{% endhint %}



### `sendMessageNoEscape` Template

As we have already mentioned earlier in case of **response**, certain mentions such as role mentions, `@everyone` and `@here` are escaped by default. This is also valid for `sendMessage` template as well. However if you want these mentions to be not escaped you should use `sendMessageNoescape` template. It is important to note that unlike response, using mention based templates such as `mentionHere` does not create a mention with `sendMessage` template. You must use `sendMessageNoEscape` for these special mentions to work. Also, just like `sendMessage` , `sendMessageNoEscape` is a purely action based template producing no meaningful/usable output.  
  
**Example :**

```text
{{sendMessage nil "@here"}}{{sendMessage nil (mentionEveryone)}}
{{sendMessageNoEscape nil "@here"}}
```

**Output :**

![](../.gitbook/assets/image%20%285%29.png)

**Explanation :**

In the above example, as already discussed `sendMessage` template never produces a special mention no matter how the special mention is produced. The third output belongs to the `sendMessageNoEscape` template which produces a mention.



### `sendMessageRetID` and `sendMessageNoEscapeRetID` Templates

These templates function very similar  to their counterparts `sendMessage` and `sendMessageNoEscape` except that they also output the ID of the discord message posted by them during execution. Hence these two templates both perform and action and produce an output placing them under the third category of templates. This is especially useful for editing messages. The ID output by them can be simply captured by any variable just like any other template which produces an output for later use.

**Example :**

```text
First Message ID = {{sendMessageRetID nil "Yag is cool"}}
{{$ID:= sendMessageRetID nil "I know!" -}}
Second Message ID = {{$ID}}
```

**Output :**

![](../.gitbook/assets/image%20%282%29.png)

**Explanation :**

The execution order is as follows - initially the first `sendMessage` template is executed and posts a message `Yag is cool`. It outputs it's message ID which becomes a part of the response since it is not captured by a variable. Then, the second `sendMessage` template is executed and posts the second message `I know!`. The ID of this message is captured by a variable $ID later used for forming the response. At the end of program execution, response is  posted as a separate message.

{% hint style="info" %}
Note: These templates output an ID only if the bot was successful in posting a message. If it was unable to post an output, due to bot lacking permissions or invalid channel id passed, it produces no meaningful output just like `sendMessage` .   
The output in these cases is simply an empty string "" which corresponds to the zero value of String datatype! Interestingly, all pure action based templates also output an empty string.
{% endhint %}

## Editing Messages

