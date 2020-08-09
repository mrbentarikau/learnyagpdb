---
description: >-
  In this page you will learn about some more composite datatypes available in
  YAGPDB templating system.
---

# Data Types 2

This page will mainly deal with _custom data types_ in YAGPDB templating system. A **composite data type** or **compound data type** is any data type which can be constructed in a program using the programming language's primitive data types and other composite types. A **custom datatype** is one which is defined by the programmer and is not originally present in the language. They are usually derived from already available data types. 

Now with all the terminology out of our way let us explore a few basic concepts which will help us understand the _custom data types_ in YAGPDB. 

## General Composites:

### 1\) Arrays : 

An array is a numbered sequence of elements of a single type with a fixed length. Arrays start at index/position 0 and hence for a array of length n, the indices very from 0 to n-1. 

Example: 

`["one" "two" "another"]`

 Consider the above array of strings with 3 elements : `"one"` `"two"` and `"another"`. Since it has 3 elements, the length of the array is 3. The first element `"one"` has the index `0` while the last element `"another"`  has index `2`. We will explore the importance of indices shortly.

### 2\) Slices :

A slice by definition is a segment of an array with flexible length. In more plain words, a slice just like an array is a  numbered sequence of elements of a single type which are indexable and has a length. However, unlike an array, this length is flexible. You can increase the length \(or size\) of a slice by adding elements to it or even decrease its size by removing elements from it. In YAGPDB templating system we will deal mostly with slices and hence we are free to add more elements or remove elements from the collection.

### 3\) Maps :

A map is an unordered collection of key-value pairs. Also known as an associative array, a hash table or a dictionary, maps are used to look up a value by its associated key. In golang the parent language on which YAGPDB templating system is based, maps keys and values must have a common type just like elements of an array. However this is something we would rarely have to concern ourselves with since the custom composite datatypes in YAGPDB templating system uses the concept of _interface{}_ data type to allow the support of multiple data types in values and in some circumstances even keys.

{% hint style="info" %}
**Extra Information :**  An interface type in Go is kind of like a _definition_. It defines and describes the exact methods that _some other type_ must have. The interface type that specifies zero methods is known as the empty interface : `interface{}`

An empty interface may hold values of any type. \(Every type implements at least zero methods.\)

Empty interfaces are used by code that handles values of unknown type. Thus in form of a high level abstraction as far as YAGPDB templating package is concerned, we can treat `interface{}` as a special data type which allow storage of any data-type of our choice thus dealing with the strict single-type limitations of slices and maps.
{% endhint %}

