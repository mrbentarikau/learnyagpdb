# Control Flow 1

Up until now we have seen how to produce some outputs and the various datatypes. We have also seen how templates can he very helpful in calculations and computations like generating the table of a number. However what if we want to output a response depending of the value inside a variable. What of we want to run a part of the code under certain conditions and another part in other situations. This is possible through control flow which we will explore in this section.

## Boolean Logic

Before exploring control flow it is important to understand boolean logic \(the mathematics of `true` and `false`\). We have seen in Datatypes 1 that boolean literals can only be of two kinds : `true` and `false`. There are 3 major operations concerning boolean datatype :

1. **NOT Operation**: This is the simplest operation concerning booleans. It simply changes `true` to `false` and vice versa. The template associated with this operation is the `not` template. It accepts a single boolean argument and returns its opposite boolean value.  eg: `{{$x := true}} {{$y := not $x}}` In this code snippet initially `true` is stored in variable x. the `not` template then performs a not operation in variable x \(which contains `true`\) returning `false`. Thus eventually `false` is stored in y. 
2. **AND Operation**: 
3. **OR Operation**: 



