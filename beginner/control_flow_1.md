# Control Flow 1

Up until now we have seen how to produce some outputs and the various datatypes. We have also seen how templates can he very helpful in calculations and computations like generating the table of a number. However what if we want to output a response depending of the value inside a variable. What if we want to run a part of the code under certain conditions and another part in other situations. This is possible through control flow which we will explore in this section.

## Boolean Logic

Before exploring control flow it is important to understand [boolean logic](https://en.wikipedia.org/wiki/Boolean_algebra#Basic_operations) \(the mathematics of `true` and `false`\). We have seen in Datatypes 1 that boolean literals can only be of two kinds : `true` and `false`. There are 3 major operations concerning boolean datatype :

### **1\) NOT Operation**: 

This is the simplest operation concerning booleans. It simply changes `true` to `false` and vice versa. The template associated with this operation is the `not` template. It accepts a single boolean argument and returns its opposite boolean value.  
  
Example: `{{$x := true}} {{$y := not $x}}` In this code snippet initially `true` is stored in variable x. the `not` template then performs a not operation in variable x \(which contains `true`\) returning `false`. Thus eventually `false` is stored in y.

### **2\) AND Operation**: 

The `and` operation is another boolean operation involving two boolean values which  results in `true` only if both the operands \( values on which it operates\) are `true` and otherwise results in `false` . The following table captures the working of the boolean `and` operation.

| Operand 1 | Operand 2 | Result |
| :--- | :--- | :--- |
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

You can use the `and` template for performing boolean `and` operation. It accepts more than two arguments. The evaluated result follows the following logic : Consider three boolean values passed to the `and` template. It finds the `and` of first and second value. Then it finds the `and` of the result from the first two values and the third value. Similar logic applies for more than 3 arguments passed the the `and` template.  
  
Example:  
`{{$x := true}} {{$y := true}} {{$z := false}}  
{{$Result := and $x $y $z}}`   
In the above example, first the `and` of variable x and y results in `true`. Then the `and` of the result and z produces `false`. Hence, finally `false` is stored in variable Result.

### 3\) OR Operation :

The `or` operation similar to `and` operation operates on two boolean literals. It results in `false` when both the operands are `false` and otherwise results in `true` .The following table captures the working of the `or` operation.

| Operand 1 | Operand 2 | operand 3 |
| :--- | :--- | :--- |
| true | true | true |
| true  | false | true |
| false  | true | true |
| false | false | false |

 The `or` template is used to perform `or` operation. Similar to `and` operator, if more than two values are passed to the `or` template, if first evaluates the result of the first two operands. Then it performs `or` operation on the result from first two operands and the third operand and so on.  
  
Example:  
`{{$x := false}} {{$y := false}} {{$z := true}}  
{{$Result := or $x $y $z}}`   
In the above example, first the o of variable x and y results in `false`. Then the `or` of the result and z produces `true`. Hence, finally `true` is stored in variable Result.

## Comparison Operators

Having seen boolean operators it is only logical to explore templates that produce boolean values as output. These Templates fall under the general category of comparison operators. The following comparison operators are available as a part of standard golang text template package:

* `eq` : This template checks for equality and returns `true` if `arg1 == arg2` , that is if both of them are equal. It is worth nothing that for equality to hold both value as well as datatype must be same. 
* `ne` : This template is the reverse of the equality template and returns `true` if `arg1 != arg2` , that is if both of them are unequal, the template returns `true`.
* `gt` : This returns `true` if `arg1 > arg2` , that is if first argument is strictly greater than second argument.
* `ge`: This returns `true` if `arg1 >= arg2` , that is if first argument is greater than or equal to second argument.
* `lt`: This returns `true` if `arg1 < arg2` , that is if first argument is strictly less than second argument.
* `le`: This returns `true` if `arg1 <= arg2` , that is if first argument is less than or equal to second argument.

{% hint style="danger" %}
Values of two **different datatypes** \(eg float64 and int\) are **not comparable**.
{% endhint %}

{% hint style="info" %}
Although it is most common to use numerical values in comparison operators, they can compare strings as well. Strings are compared using the Unicode values of their constituent runes\(codepoints\). If the first codepoints are equal, the second ones are compared and so on.
{% endhint %}

{% hint style="info" %}
Only basic datatypes \( int and variants ; float and variants and strings\) can be compared with the comparison operators. The `eq` and `ne` operators can additionally also compare boolean values.
{% endhint %}

## If - Else Branching

Equipped with the knowledge of Conditional and Boolean operators and their corresponding templates, we can now explore the very first type of control flow: if-else branching. If else branch statements in their most basic form allows you to execute a certain set of instructions or code if a certain condition is satisfied and a different set of instructions/code if it is the condition is not satisfied.  The basic syntax of an if-else template is :  


```text
{{if (condition)}}
    Statement(s) to be executed if condition is true
{{else}}
    Statement(s) to be executed if condition is false
{{end}}
```

Example:

```text
{{$a := 1}}
{{if gt $a 0}}
    Number is more than 0
{{else}}
    Number id less than 0
{{end}}
```

In the above example first the conditional operator gt checks if the variable `$a` contains a number that is more than `0` . Since this condition is satisfied gt returns `true`. Since the condition is true, the **block** of code/statements following the if  template is executed. In the above example, `Number is more than 0` is printed as output by the bot.  
  
It is important to note here that the else template along with the code to be executed if condition is false \(can be referred to as the else **block**\) is not compulsory. However, the `{{end}}` statement is compulsory and marks the end of the if-else conditional **block**.

Example:

```text
{{$name := "Peter"}}
{{if eq $name "YAGPDB"}}
    Oh another me!
{{end}}
Hello! {{$name}}
```

In the above example the if **block** will be executed only of $name is `"YAGPDB"` . Since condition is `false`, the if **block** is skipped and the bot simply prints the output : `Hello! Peter`.

### Blocks and Scope

While learning the if-else template you must have noticed the term "block". A **block** is simply a collection of statements or code. With the simple if-else template as an example, the statements following `{{if (condition}}` and before `{{else}}` \(or `{{end}}` if there is no else template\) consists of the a single block which can be called the if block. Similarly the statements following `{{else}}` and before `{{end}}` consists of the else block. 

So why are these blocks important anyway? They are important because of an important property of variables called **scope**. A variable declared in a particular block ceases to exist outside it. This can be illustrated as follows:  
`{{if eq 1 1}}  
    {{$a := 1}}  
{{end}}  
{{$a}}`    
This code will generate an error because the variable $a was defined inside the if block and ceases to exist after the `{{end}}` statement. It is very important to keep a track on a variable's scope while writing codes to avoid such errors. 

{% hint style="info" %}
Defining a variable which already exists makes a local copy of that variable which exists within that block while the outer version is not overwritten and comes back into existence outside the block. This can be avoided by using assignment operator `=` instead.  
Example:  
 `{{$a := 1}}{{$b:= 2}}  
       {{if eq $b 2}}  
           {{$a := 3}}{{$a}}  
{{end}}  
{{$a}}`

The above code will Output:  
`3  
  
1`
{% endhint %}



