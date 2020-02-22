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
  
It is important to note that `sendMessage` template by itself is an **action type** template , that is it does not produce a meaningful output by itself. It simply directs the bot to send the content that is passed to it in a **separate message** from the custom command's default **response** message.

#### Example 1:

`{{sendMessage nil "Hello World"}}`  
  
**Output:**

![](../.gitbook/assets/image%20%2814%29.png)

**Explanation:**  
As discussed earlier, the `sendMessage` template here simply sends the string as output message in the same channel as the one in which the custom command is running.

#### Example 2:

`This is the response message{{sendMessage nil "This is a separate message"}}!`

**Output:**  


![](../.gitbook/assets/image%20%285%29.png)

**Explanation :**  
  
As discussed earlier, the `sendMessage` template sends a **separate message** as output which is different from the custom command's default response message. The `sendMessage` template being an action type template producing no meaningful output is also shown in the above example. That is the reason why the `!` appears directly after `message` in the response message although if you look at the original code the `sendMessage` template was placed between them.

{% hint style="info" %}
In the previous example it is important to note that the `sendMessage` template's output message is sent before the custom command's response message. The reason for this is that the response is generated only after the execution of the custom command is finished however the `sendMessage` template directs the bot to send a message during its execution.  
Example:   
`third{{sendMessage nil "first"}}{{sendMessage nil "second"}}`  
Output:  
 ![](../.gitbook/assets/image%20%2813%29.png)
{% endhint %}



