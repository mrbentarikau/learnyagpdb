# Control Flow 1

Up until now we have seen how to produce some outputs and the various data types. We have also seen how templates can be very helpful in calculations and computations like generating the table of a number. However what if we want to output a response depending of the value inside a variable. What if we want to run a part of the code under certain conditions and another part in other situations. This is possible through control flow which we will explore in this section.

## Boolean Logic

Before exploring control flow it is important to understand [boolean logic](https://en.wikipedia.org/wiki/Boolean_algebra#Basic_operations) \(the mathematics of `true` and `false`\). We have seen in Datatypes 1 that boolean literals can only be of two kinds : `true` and `false`. There are 3 major operations concerning boolean datatype :

### **1\) NOT Operation**: 

This is the simplest operation concerning booleans. It simply changes `true` to `false` and vice versa. The template associated with this operation is the `not` template. It accepts a single boolean argument and returns its opposite boolean value.  
  
Example : `{{$x := true}} {{$y := not $x}}` In this code snippet initially `true` is stored in variable `$x`. the `not` template then performs a not operation in variable `$x` \(which contains `true`\) returning `false`. Thus eventually `false` is stored in $y.

### **2\) AND Operation**: 

The `and` operation is another boolean operation involving two boolean values which  results in `true` only if both the operands \( values on which it operates\) are `true` and otherwise results in `false` . The following table captures the working of the boolean `and` operation.

| Operand 1 | Operand 2 | Result |
| :--- | :--- | :--- |
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

You can use the `and` template for performing boolean `and` operation. It accepts more than two arguments. The evaluated result follows the following logic: Consider three boolean values passed to the `and` template. It finds the `and` of first and second value. Then it finds the `and` of the result from the first two values and the third value. Similar logic applies for more than 3 arguments passed the the `and` template.  
  
Example :  
`{{$x := true}} {{$y := true}} {{$z := false}}  
{{$Result := and $x $y $z}}`   
In the above example, first the `and` of variables `$x` and `$y` results in `true`. Then the `and` of the result and `$z` produces `false`. Hence, finally `false` is stored in variable Result.

### 3\) OR Operation :

The `or` operation similar to `and` operation operates on two boolean literals. It results in `false` when both the operands are `false` and otherwise results in `true` . The following table captures the working of the `or` operation.

| Operand 1 | Operand 2 | operand 3 |
| :--- | :--- | :--- |
| true | true | true |
| true  | false | true |
| false  | true | true |
| false | false | false |

 The `or` template is used to perform `or` operation. Similar to `and` operator, if more than two values are passed to the `or` template, if first evaluates the result of the first two operands. Then it performs `or` operation on the result from first two operands and the third operand and so on.  
  
Example :  
`{{$x := false}} {{$y := false}} {{$z := true}}  
{{$Result := or $x $y $z}}`   
In the above example, first the o of variable `$x` and `$y` results in `false`. Then the `or` of the result and `$z` produces `true`. Hence, finally `true` is stored in variable Result.

## Comparison Operators

Having seen boolean operators it is only logical to explore templates that produce boolean values as output. These Templates fall under the general category of comparison operators. The following comparison operators are available as a part of standard golang text template package:

* `eq` : This template checks for equality and returns `true` if `arg1 == arg2` , that is if both of them are equal. It is worth nothing that for equality to hold both value as well as data type must be same. 
* `ne` : This template is the reverse of the equality template and returns `true` if `arg1 != arg2` , that is if both of them are unequal, the template returns `true`.
* `gt` : This returns `true` if `arg1 > arg2` , that is if first argument is strictly greater than second argument.
* `ge`: This returns `true` if `arg1 >= arg2` , that is if first argument is greater than or equal to second argument.
* `lt`: This returns `true` if `arg1 < arg2` , that is if first argument is strictly less than second argument.
* `le`: This returns `true` if `arg1 <= arg2` , that is if first argument is less than or equal to second argument.

{% hint style="danger" %}
Values of two **different data types** \(eg float64 and int\) are **not comparable**.
{% endhint %}

{% hint style="info" %}
Although it is most common to use numerical values in comparison operators, they can compare strings as well. Strings are compared using the [Unicode values](https://www.tamasoft.co.jp/en/general-info/unicode-decimal.html) of their constituent runes\(codepoints\). If the first codepoints are equal, the second ones are compared and so on.
{% endhint %}

{% hint style="info" %}
Only basic data types \(int and variants ; float and variants and strings\) can be compared with the comparison operators. The `eq` and `ne` operators can additionally also compare boolean values.
{% endhint %}

## If - Else Branching

Equipped with the knowledge of Conditional and Boolean operators and their corresponding templates, we can now explore the very first type of control flow: if-else branching. If else branch statements in their most basic form allows you to execute a certain set of instructions or code if a certain condition is satisfied and a different set of instructions/code if it is the condition is not satisfied.  The basic syntax of an if-else template is :

```go
{{if (condition)}}
    Statement(s) to be executed if condition is true
{{else}}
    Statement(s) to be executed if condition is false
{{end}}
```

Example :

```go
{{$a := 1}}
{{if gt $a 0}}
    Number is more than 0
{{else}}
    Number is less than 0
{{end}}
```

In the above example first the conditional operator gt checks if the variable `$a` contains a number that is more than `0` . Since this condition is satisfied gt returns `true`. Since the condition is true, the **block** of code/statements following the if  template is executed. In the above example, `Number is more than 0` is printed as output by the bot.  
  
It is important to note here that the else template along with the code to be executed if condition is false \(can be referred to as the else **block**\) is not compulsory. However, the `{{end}}` statement is compulsory and marks the end of the if-else conditional template.

Example :

```go
{{$name := "Peter"}}
{{if eq $name "YAGPDB"}}
    Oh another me!
{{end}}
Hello! {{$name}}
```

In the above example the if **block** will be executed only of `$name` is `"YAGPDB"` . Since condition is `false`, the if **block** is skipped and the bot simply prints the output : `Hello! Peter`.

### Blocks and Scope

While learning the if-else template you must have noticed the term "block". A **block** is simply a collection of statements or code. With the simple if-else template as an example, the statements following `{{if (condition}}` and before `{{else}}` \(or `{{end}}` if there is no else template\) consists of the a single block which can be called the if block. Similarly the statements following `{{else}}` and before `{{end}}` consists of the else block. 

So why are these blocks important anyway? They are important because of an important property of variables called **scope**. A variable declared in a particular block ceases to exist outside it. This can be illustrated as follows:  
`{{if eq 1 1}}  
    {{$a := 1}}  
{{end}}  
{{$a}}`    
This code will generate an error because the variable $a was defined inside the if block and ceases to exist after the `{{end}}` statement. It is very important to keep a track on a variable's scope while writing codes to avoid such errors. 

{% hint style="info" %}
Defining a variable which already exists makes a local copy of that variable which exists within that block while the outer version is not overwritten and comes back into existence outside the block. This can be avoided by using assignment operator = instead.  
Example :  
 `{{$a := 1}}{{$b:= 2}}  
       {{if eq $b 2}}  
           {{$a := 3}}{{$b = 2}}{{$a}},{{$b}}  
{{end}}  
{{$a}},{{$b}}`  
The above code will Output:  
`3,2  
  
1,2`
{% endhint %}

### If - Else If - Else Branching

Earlier we have seen how to execute a block of statements if condition is true and another block if it is false. However if we have multiple conditions in that case the if - else if - else branching is very helpful. This is often called if-else chaining. The general syntax is :

```go
{{if (condition_1)}}
    If block
{{else if (condition_2)}}
    Else If  Block_1
{{else if (condition_3)}}
    Else If Block_2
{{else}}
    Else Block
{{end}}
```

Note : you are not limited to only two else-if blocks but they can be as many as you want. The final else block can also be skipped if not necessary just like the previous case however the `{{end}}` statement is necessary to mark the end of the branching template.  
  
Example:

```go
{{$marks := 95}}
{{if gt $marks 90}}
    You Passed with Disctiction.
{{else if gt $marks 35}}
    You Passed.
{{else}}
    You Failed.
{{end}}
```

In the above example there are 3 different blocks which are conditionally executed depending on the value of $marks. For the above example, `You Passed with Disctiction` will be printed as output by bot.

## Example Codes

### Example 1

```go
{{if .User.Bot}}
   The newly joined account is a bot!
{{else}}
    Hello there {{.User.Mention}}! Welcome to our server {{.Server.Name}}.
    
    {{- if eq .Server.MemberCount 100 -}}
        You are our 100th member!!
    {{- else if eq .Server.MemberCount 500 -}}
        You are our 500th member!!
    {{- else if eq .Server.MemberCount 1000 -}}
        You are our 500th member!!
    {{end}}
{{end}}
```

Above is an example code which can be used in [Join Message](https://docs.yagpdb.xyz/notifications-and-feeds/notification-feed#general-feed). \(Note for normal custom commands, bots can't trigger them\).  
  
There are two major takeaways from the above example. Firstly notice how an if-else or if-else if-else statement can be used inside another block. In this case it is executed only of the first condition is false for the outer if statement \(that is user joining is not a bot\). This is called nesting and can be very useful for checking a complex set of conditions. Secondly notice the `-` at use in the internal if-else if-else templates. It is used to trim spaces to the left and right of the templates to aid with formatting. It is discussed in more detail [here](https://golang.org/pkg/text/template/#hdr-Text_and_spaces).

#### Output:

Sample output for normal user joining :

![](../../.gitbook/assets/image.png)

Sample output for bot user joining :

![](../../.gitbook/assets/image%20%282%29.png)



### Example 2

```go
{{if .Message.Attachments}}
    This message has an attachment!
{{end}}
```

Above snippet will print `This message has an attachment` if triggering message has an attachment. Notice how in this example we are using .Message.Attachments which is not of boolean data type as the if statement's condition. This is possible because non boolean variables are automatically converted to boolean when used in a condition according to the following logic :   
If the the data represents the **zero value**\( nil or empty, zero  or false depending on data type\) of the associated data type, it is treated as `false` and otherwise considered as `true`. This helps in determining if a certain value is nil or empty \(or number is zero\) very efficient.  


{% hint style="success" %}
Pro Tip:  
Existence of multiple data values can be determined by using the Boolean operators. The logic follows the same logic as for boolean literals after the data values are converted to `true` or `false` depending on their stored value and datatype.  
Example:  
`{{if or .Message.Attachments .Message.Embeds}}  
     This is not a simple text message   
{{end}}`  
The above example prints `This is not a simple text message` if  the triggering message consists any attachments or embeds.
{% endhint %}



