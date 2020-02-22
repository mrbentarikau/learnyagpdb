# Outputs 1

## Response

With the exception of content enclosed in two brackets `{{ template }}` which denotes a template to be "compiled", any extraneous content which is not enclosed in two brackets will be sent without any modification. This output message is called the custom command **response.** 

{% hint style="info" %}
The response is sent in the same channel that the custom command is triggered.
{% endhint %}

**Example:**

```go
Hello World. 
```

The above will make the bot print out "Hello World." in form of a response message when the command gets **triggered**.

![](../../.gitbook/assets/image%20%286%29.png)

Practice Question: Write a command to output : This is **yag bot**

## Template

Things written inside double curly brackets `{{ tempalate goes here }}` are called templates. We have seen earlier how we can output specific textual response after invoking a custom command. However, sometimes we want the output to change depending in the person running the command, the channel it is run on etc. Templates help us in such situations. Templates can also be used for much more complex behavior which we will slowly explore over the subsequent sections. Templates can basically produce three kinds of actions/effects:

1.  **Produce an output :** Certain templates simply produce an output. This output becomes a part of the response unless directed to another template or variable. The most common templates of this kind are the "property templates" which returns information about the current Server/Channel/Member etc. The property templates always start with a `.` and if they have sub properties \( like current server is the main structure and it has sub fields like Name , ID etc.\), there is a `.` between the main struct and it's sub fields.  example: `{{.Server.Name}}` outputs the name of your server. `{{.CCID}}` is the numerical ID of the current custom command. Some other examples are `{{.User.Username}}` , `{{.Channel.Name}}` so on. More can be found in the docs \([\[1\]](https://docs.yagpdb.xyz/reference/templates#guild-server) , [\[2\]](https://docs.yagpdb.xyz/reference/templates#channel) , [\[3\]](https://docs.yagpdb.xyz/reference/templates#message) , [\[4\]](https://docs.yagpdb.xyz/reference/templates#member) , [\[5\]](https://docs.yagpdb.xyz/reference/templates#user)\).  
2.  **Perform an action :** These templates do not return any meaningful data; they simply perform an action.  
3. **Both perform an action + produce an output :**  These templates perform a computation/calculation/action and also produce an output at the end. example: `{{add 1 2}}` adds two numbers and returns their sum.

## Example Codes 

### Example 1

```text
Hey there {{.User.Username}}!
Welcome to our Server : {{.Server.Name}}
```

#### Output :

![](../../.gitbook/assets/image%20%2810%29.png)

### Example 2

```text
This is <#{{.Channel.ID}}> {{mentionEveryone}}
```

#### Explanation :

In the above code we explore how to produce mentions. Simply writing @user or @role or \#channel will _**not**_ produce a user , channel or role mention. Mentions follow a specific format which is elaborated [here](https://docs.yagpdb.xyz/reference/templates#mentions). Also, by default role mentions will be escaped\(not produce a mention\). You will have to use special templates to produce a role mention as seen in the above code with `{{mentionEveryone}}` .

#### Output :

![](../../.gitbook/assets/image%20%288%29.png)

\*\*\*\*

{% hint style="success" %}
**Pro Tip :** Did you know you can combine multiple templates such that the output of one template serves as input to another. We can achieve this by enclosing the interior template within parenthesis `()`

**Example :**  `{{mult 2 (add 3 2)}}`  
Produces 10 as output.
{% endhint %}



