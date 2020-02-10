# Control Flow 1

Up until now we have seen how to produce some outputs and the various datatypes. We have also seen how templates can he very helpful in calculations and computations like generating the table of a number. However what if we want to output a response depending of the value inside a variable. What if we want to run a part of the code under certain conditions and another part in other situations. This is possible through control flow which we will explore in this section.

## Boolean Logic

Before exploring control flow it is important to understand boolean logic \(the mathematics of `true` and `false`\). We have seen in Datatypes 1 that boolean literals can only be of two kinds : `true` and `false`. There are 3 major operations concerning boolean datatype :

### **1\) NOT Operation**: 

This is the simplest operation concerning booleans. It simply changes `true` to `false` and vice versa. The template associated with this operation is the `not` template. It accepts a single boolean argument and returns its opposite boolean value.  
  
eg: `{{$x := true}} {{$y := not $x}}` In this code snippet initially `true` is stored in variable x. the `not` template then performs a not operation in variable x \(which contains `true`\) returning `false`. Thus eventually `false` is stored in y.

### **2\) AND Operation**: 

The `and` operation is another boolean operation involving two boolean values which  results in `true` only if both the operands \( values on which it operates\) are `true` and otherwise results in `false` . The following table captures the working of the boolean `and` operation.

| Operand 1 | Operand 2 | Result |
| :--- | :--- | :--- |
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

You can use the `and` template for performing boolean `and` operation. It accepts more than two arguments. The evaluated result follows the following logic : Consider three boolean values passed to the `and` template. It finds the `and` of first and second value. Then it finds the `and` of the result from the first two values and the third value. Similar logic applies for more than 3 arguments passed the the `and` template.  
  
eg:  
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
  
eg:  
`{{$x := false}} {{$y := false}} {{$z := true}}  
{{$Result := or $x $y $z}}`   
In the above example, first the o of variable x and y results in `false`. Then the `or` of the result and z produces `true`. Hence, finally `true` is stored in variable Result.

## Comparison Operators

Having seen boolean operators it is only logical to explore templates that produce boolean values as output. These Templates fall under the general category of comparison operators. The following comparison operators are available as a part of standard golang text template package:

* `eq` : This template checks for equality and returns `true` if `arg1 == arg2` , that is if both of them are equal. It is worth nothing that for equality to hold both value as well as datatype must be same. Values of two different datatypes \(eg float64 and int\) are not comparable.
* `ne` : This template is the reverse of the equality template and returns `true` if `arg1 != arg2` , that is if both of them are unequal, the template returns `true`.
* `gt` : This returns `true` if `arg1 > arg2` , that is if first argument is strictly greater than second argument.
* `ge`: This returns `true` if `arg1 >= arg2` , that is if first argument is greater than or equal to second argument.
* `lt`: This returns `true` if `arg1 < arg2` , that is if first argument is strictly less than second argument.
* `le`: This returns `true` if `arg1 <= arg2` , that is if first argument is less than or equal to second argument.

{% hint style="info" %}
Although it is most common to use numerical values in comparison operators, they can compare strings as well. Strings are compared using the Unicode values of their constituent runes\(codepoints\). If the first codepoints are equal, the second ones are compared and so on.
{% endhint %}

{% hint style="info" %}
Only basic datatypes \( int and variants ; float and variants and strings\) can be compared with the comparison operators. The `eq` and `ne` operators can additionally also compare boolean values.
{% endhint %}


