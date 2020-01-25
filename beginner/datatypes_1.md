# Datatypes 1

## Basic Datatypes

### String

A string is a sequence of characters. It simply stores textual data. String literals can be defined in two ways:

1.  **Using Double Quotes :**  String literals can be created by enclosing a sequence of characters within double quotation marks `"` . It can cannot contain newlines and allows usage of special escape sequences to represent certain characters. Some of them are :  `\n` This produces a newline character \(similar to pressing the enter key\) `\"` This produces a double quotation mark `"` . This allows us to use double quotes inside quoted       string literals. `\\` This creates a backslash character. \(only using a single backslash character denotes an escape sequence hence this is necessary\)  A more detailed list of other escape sequences can be found [here](http://xahlee.info/golang/golang_string_backslash_escape.html). Example : `"Yagpdb is a nice bot.\nI like it very much."` 
2. **Using Backticks :** String literals can also be created in form of a _raw string literal_ by enclosing it in backticks `````. It can contain all characters including newlines except for the backtick character. It does not support any escape sequences and is usually used to conveniently produce string literals which span multiple lines.  Example :  ```Yagpdb is a nice bot. I like it very much```

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



