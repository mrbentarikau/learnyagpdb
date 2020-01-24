# Outputs 1

## Response

Most characters typed into the response box of a custom command \(unless following the specific template format discussed later\) will be outputted as is without any modification in form of a message by the bot. This output message is called the custom command **response.** 

{% hint style="info" %}
The response is sent in the same channel that the custom command is triggered.
{% endhint %}

**Example:**

```go
Hello World. 
```

The above will make the bot print out "Hello World." in form of a response message when the command gets **triggered**.

![](../../.gitbook/assets/image.png)

Practice Question: Write a command to output : This is **yag bot**

## Template

Things written inside double curly brackets `{{ tempalate goes here }}` are called templates. This structure can basically produce three kind of actions:

1.  Produce an output: Certain templates simply produce an output.  example: {{.Server.Name}} outputs the name of your server. Some other examples are {{.User.Username}} , {{.Channel.Name}} so on. More can be found in the [docs](https://docs.yagpdb.xyz/reference/templates#user). 
2.  Perform an action. \( More on this later\)
3. Both perform an action + produce an output. example: {{add 1 2}} adds two numbers and returns their sum.

## Example Code 

```text
Hey there {{.User.Username}}!
Welcome to our Server : {{.Server.Name}}
```

#### Example output :

![](../../.gitbook/assets/image%20%281%29.png)

{% hint style="success" %}
**Pro Tip:** Did you know you can combine multiple templates such that the output of one template serves as input to another. We can achieve this by enclosing the interior template within parenthesis \(\)

**Example: `{{mult 2 (add 3 2)}}`**  
Produces 10 as output.
{% endhint %}



