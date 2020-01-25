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

![](../../.gitbook/assets/image%20%283%29.png)

Practice Question: Write a command to output : This is **yag bot**

## Template

Things written inside double curly brackets `{{ tempalate goes here }}` are called templates. This structure can basically produce three kind of actions:

1.  Produce an output: Certain templates simply produce an output.  example: {{.Server.Name}} outputs the name of your server. Some other examples are {{.User.Username}} , {{.Channel.Name}} so on. More can be found in the docs \([\[1\]](https://docs.yagpdb.xyz/reference/templates#guild-server) , [\[2\]](https://docs.yagpdb.xyz/reference/templates#channel) , [\[3\]](https://docs.yagpdb.xyz/reference/templates#message) , [\[4\]](https://docs.yagpdb.xyz/reference/templates#member) , [\[5\]](https://docs.yagpdb.xyz/reference/templates#member)\). 
2.  Perform an action \( _More on this later_ \)
3. Both perform an action + produce an output  : These templates perform a computation/calculation/action and also produce an output at the end. example: {{add 1 2}} adds two numbers and returns their sum.

## Example Code 

### Example 1

```text
Hey there {{.User.Username}}!
Welcome to our Server : {{.Server.Name}}
```

#### Output :

![](../../.gitbook/assets/image%20%287%29.png)

### Example 2

```text
This is <#{{.Channel.ID}}> {{mentionEveryone}}
```

#### Explanation :

In the above code we explore how to produce mentions. Simply writing @user or @role or \#channel will _not_ produce a user , channel or role mention. Mentions follow a specific format which is elaborated [here](https://docs.yagpdb.xyz/reference/templates#mentions). Also, by default role mentions will be escaped\(not produce a mention\). You will have to use special templates to produce a role mention as seen in the above code with `{{mentionEveryone}}` .

#### Output :

![](../../.gitbook/assets/image%20%285%29.png)

\*\*\*\*

{% hint style="success" %}
**Pro Tip:** Did you know you can combine multiple templates such that the output of one template serves as input to another. We can achieve this by enclosing the interior template within parenthesis \(\)

**Example: `{{mult 2 (add 3 2)}}`**  
Produces 10 as output.
{% endhint %}



