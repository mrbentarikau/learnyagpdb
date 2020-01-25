# Datatypes 1

## Basic Datatypes

### String

A string is a sequence of characters. It simply stores textual data. String literals can be created in two ways:

1.  **Using Double Quotes :**  String literals can be created by enclosing a sequence of characters within double quotation marks `"` . It can cannot contain newlines and allows usage of special escape sequences to represent certain characters. Some of them are :  `\n` This produces a newline character \(similar to pressing the enter key\) `\"` This produces a double quotation mark `"` . This allows us to use double quotes inside quoted       string literals. `\\` This creates a backslash character. \(only using a single backslash character denotes an escape sequence hence this is necessary\)  A more detailed list of other escape sequences can be found [here](http://xahlee.info/golang/golang_string_backslash_escape.html). Example : `"Yagpdb is a nice bot.\nI like it very much."` 
2. **Using Backticks :** String literals can also be created in form of a _raw string literal_ by enclosing it in backticks `````. It can contain all characters including newlines except for the backtick character. It does not support any escape sequences and is usually used to conveniently produce string literals which span multiple lines.  Example :  ```Yagpdb is a nice bot. I like it very much```

### Integer

Integers \(_int_\) – like their mathematical counterpart – are numbers without a decimal component. In Yagpdb templating code, the maximum range of int datatype is from : -9223372036854775808 to 9223372036854775807. There are [different ways](https://golang.org/ref/spec#Integer_literals) in which an integer literal can be created/specified but irrespective of how they are specified, they all belong to the same datatype and represent an unique number. Some common ways are :

1. **As base 10 number :** As intimidating as it sounds, these are our normal plain numbers. So normal digits can be used to create number literals \(remember that the first digit should be non zero for syntax reasons\).  `$x := 105` Above statement assigns a variable named x with value 105 \(base-10\) 
2. **As a hexadecimal number :** You might have come across [hexadecimal numbers](https://simple.wikipedia.org/wiki/Hexadecimal) while reading about memory locations or hexadecimal codes for colors etc. While specifying a hexadecimal number, we have to precede the number with `0x` to denote that the following number represents a hexadecimal number. You can use digits from `0` to `9` and letters `a` to `e` to specify a hexadecimal number. Capitalization of the letters no not matter. `$hex := 0xA1` Above statement assigns a variable named hex with value : 161\(base-10\) using an integer literal specified in hexadecimal format.

{% hint style="info" %}
Preceding an integer literal with 0 makes the compiler interpret it as a number specified in [octal](https://simple.wikipedia.org/wiki/Octal) notation \(base-8\).   
e.g. : `$x := 011`   
stores 9 \(base-10\) in variable named x and not 11. In fact, `9` is written as `11` in octal notation.
{% endhint %}

{% hint style="info" %}
_int64_ is another datatype which is very similar to _int_ but is always 64 bits size irrespective of compiler. int64 can be converted to int using the `toInt` template. Reverse can be achieved using `toInt64` template.

e.g. :  `$num := toInt64 105`  
Stores 105 \(base-10\) in variable called num but as _int64_ datatype and not _int_.  
By default however \(without explicit `toInt64` conversion\) Integer literals are stored as _int_ datatype.
{% endhint %}

### Float

Floating point numbers are numbers that contain a decimal component \(real numbers\). They are specified with a number with a decimal point.   
eg : `9.5` `12.3` `0.008`

Floating point literals also support some other formats such as scientific notation etc. elaborated [here](https://golang.org/ref/spec#Floating-point_literals).

{% hint style="info" %}
Note `10` represents an integer literal while `10.0` represents a floating point literal.  
e.g. : `$num := 20.0`   
Stores 20.0 \(base-10\) in a variable called num with datatype _float64_ and not _int_. 
{% endhint %}

{% hint style="info" %}
template `toFloat` can be used to convert int to _float64_. reverse can be achieved via `toInt` template. However when a float is converted to integer, the decimal part is stripped in place of rounding it to nearest integer.  
e.g. : `$x := toInt 12.98`   
In the above statement, 12 \(base-10\) is stored in the variable named x and not 13. 
{% endhint %}

{% hint style="warning" %}
Unless otherwise specified, all numbers \(integers/float\) will be base-10 by default in the remaining sections of this website.
{% endhint %}

## Variables

A variable is a storage location, with a specific type and an associated name. It can be used to store the output of a template or literal values\( string , int , float etc\).  Names must start with a letter and may contain letters, numbers or the `_` \(underscore\) symbol. In Custom Command codes, all variable names should be preceded by the dollar sign `$` to identify it as a variable. A template containing just the variable name simply outputs it's contents \(for complex datatypes it follows certain predefined formats\).

```text
{{$name1 := "Satty"}} {{$favourite_number1 := 1}}
{{$name2 := "Yagpdb"}} {{$favourite_number2 := -1}}
{{$fun := "1\n2\n3\nDone printing code."}} 
{{$name1}} : {{$favourite_number1}}
{{$name2}} : {{$favourite_number2}}
{{$fun}}
```

The output is :

`Satty : 1  
Yagpdb : -1  
1  
2  
3  
Done printing code`

{% hint style="info" %}
Note: All preceding and trailing white spaces \(eg: space, newlines \) are always trimmed away in final output matching discord behavior.
{% endhint %}



