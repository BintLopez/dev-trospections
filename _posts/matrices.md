---
layout: post
title:  The Mindblowing Matrix
date:   2015-06-30 22:43:00
categories: rubyconf matrices
---

##Matrices Talk
* Matrices are pretty much just multidimensional arrays
* Vectors are pretty much arrays

###What is a matrix?
Array of elements in rows and columns.
composed of vectors
over a field
* an array of vectors over a field organized into rows and columns

The matrix library is part of the ruby standard library

###When to use a matrix?
* a way to structure data in order to emphasize relationships
* useful tool for solving systems of linear equations
* data structure for procedures in algorithms
* they're used in graphics in computing to manipulate polygons
* probability distributions -- markov matrix
* cryptography
* can visualize and understand relational data in matrices
* any time you're using linear equations to model a problem

Fields
a collection of numbers that have the properties of +-*/
in the field of real numbers, there are infinite numbers in the field
* good example of a field -- 'finite field' GF(2) Galoit(sp?) field -- binary 0 and 1

Vectors
* a list of scalars over a specific field
* components of matrices


Matrices
require 'matrix'
Vector[element, element]

```
#Matrix can be represented in a hash
{
	:row_one => [1,4,6]
	:row_two => [5,5,6]
	:row_three => [5,7,3]
}
```

.zip
.inverse

Calculating edit distance between two strings
distance score is a function of how many edits occurred between source and target strings

Levenshtein distance steps

Multidimentional arrays / matrices not always the most performant
O(m*n)

Color gem -- for color math
but having performance issues
conf attendee would like some help w/ it
