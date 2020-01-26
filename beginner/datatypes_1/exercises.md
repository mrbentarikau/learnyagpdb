# Exercises

1. Write a program to find the datatype and value of the following variables :

   1.  `{{$x := 10.0}}`
   2.  `{{$x := .Server.MemberCount}}`
   3.  `{{$x := 0xAB11615E}}`
   4.  `{{$x := .Channel.name}}`
   5.  `{{$x := div 5 2.3}}`
   6.  `{{$x := currentTime}}`
   7.  `{{$x := .Member.Roles}}`
   8.  `{{$x := .User}}`
   9.  `{{$x := "ab\nc\\\"\n\\"}}`
   10. 
                ``{{$x := ` ok  
   hi  
   hello  
   bye`}}``

2. Predict the output of of the following code snippet : `{{$x := mult 3 2.5}} {{$y := div 10 $x}} {{$y1 := fdiv 10 $x}} {{$z := sub 5 $y}} {{$a := div 18.0 $z}} {{$x}} , {{$y}} , {{$y1}} , {{$z}} , {{$a}}`  
3. Predict the output of of the following code snippet :  
   `{{$int := 015}} {{$value := fdiv $int (sub $int 1)}}   
   {{$value_int := add (toInt $value) 0xa05}}  
   {{$int}}, {{$value}}, {{$value_int}}`

4. Write a program to compute the radius of a circle given Area is 154 square units.  Formula : Area = Pi \* \(r^2\) ; assume Pi = 22/7.  Also, find the perimeter of the circle divided by 3. \(Perimeter = 2\*Pi\*r\) How will you ensure that the answer is in form of a decimal number ? 
