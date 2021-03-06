---
layout: lesson
root: .
title: Short Introduction to Programming in Julia
---

# The Basics of Julia

[Julia][jl] is a general purpose programming language geared towards scientific
applications, that supports rapid development of scripts and applications.

[jl]: http://julialang.org/

Julia's main advantages:

* Free and Open Source multi-platform software
* Extremely performant
* Supports multiple programming paradigms
* Very active and friendly community, outstanding documentation
* Built-in parallel and distributed computing
* Really good mathematical function library
* Built-in package manager
* Can call C and Python functions
* Full utf-8 support: `2π` is a valid expression

## Interpreter and just-in-time compilation

Julia works as an interpreted languages, but does *just-in-time* compilation:
the first time an expression is called, it will be compiled (without you
noticing), which will make the subsequent calls orders of magnitude faster.
Julia can be used in one of two ways.

* Using interpreter as an "advanced calculator" in interactive mode:

``` julia
[user@host ~]$ julia
julia> 2 + 2
4
julia> println("Hello World")
Hello World
```

* Executing programs/scripts saved as a text file, usually with `*.jl` extension:

```
[user@host ~]$ julia helloworld.jl
Hello World
```


## Introduction to Julia built-in data types

The type of variables in Julia is extremely important, as it allows a number of
performance optimizations. You can know the type of a variable with `typeof`.

### Numeric types

There are a lot of numeric types in Julia. Most of the time, we will deal with
either `Int`  or `Float`:

~~~ julia
julia> typeof(2)
Int64

julia> typeof(2.0)
Float64
~~~

Julia will let you manipulate objects of different types seamlessly:

~~~ julia
julia> 2.0 + 2
4.0
~~~

### Strings and chars

Strings and characters are different objects in Julia. Strings are delimited by `"`,
and chars by `'`.

```julia
string = "Data Carpentry"
char = 'c'
```

Here we've assigned data to variables, namely `string` and `char`, using the
assignment operator `=`.

To print out the value stored in a variable we can simply type the name of the
variable into the interpreter:

``` julia
julia> string
"Data Carpentry"
```

but this only works in the interpreter. In scripts we must use the `println` function:

``` julia
# Single-line comments start with #
# Next line will print out text
println(text)
```

### Unicode

Julia knows unicode (utf-8) well. What it means, is that you can use symbols in
your code. The following code, for example, is valid:

``` julia
α = 2
α ∈ [1 2 3]
```

We assign a value of 2 to the variable `α`, then use the `∈` symbol (which is a
shortcut for `in`) to test whether 2 is in the list of values. This code will
return `true`.

The full list of valid characters is in the [Julia documentation][jldoc]. To
write a unicode character, you need to first type in `\`, then the first few
letters of the symbol name, then press the Tab key (on most keyboards,
immediately to the left of the letter in the top left corner). This triggers
*auto-completion*: Julia will suggest a list of possible symbols.

[jldoc]: http://docs.julialang.org/en/release-0.4/manual/unicode-input/

### Sequential types: arrays and tuples

### Arrays

**Arrays** are a common data structure to hold a sequence of elements alongside
different dimensions. Elements can be accessed with their index:

``` julia
julia> numbers = [1, 2, 3]
3-element Array{Int64,1}:
 1
 2
 3

julia> numbers[1]
1
```

In Julia, indexes start at 1 (like in R), and not 0 (like in Python or C).

Note that the type of `numbers` is `Array{Int64,1}`. This communicates three
important pieces of information. First, this object is an array. Second, it
holds values with the type `Int64`. Finally, it as `1` dimension. In Julia,
arrays are "column-major", so an object with one dimension is a column vector.

Compare with:

``` julia
julia> numbers_row = [1 2 3]
1x3 Array{Int64,2}:
 1  2  3
```

This new object has two dimensions: three columns, and one row. We can easily
write matrices (two-dimensional arrays) in-line:

``` julia
julia> eye_one = [1 0 0; 0 1 0; 0 0 1]
3x3 Array{Int64,2}:
 1  0  0
 0  1  0
 0  0  1
```

Two-dimensional arrays are accessed by line first, and by column second:

``` julia
julia> eye_one[1, 3] = 1
1

julia> eye_one
3x3 Array{Int64,2}:
 1  0  1
 0  1  0
 0  0  1
```

To add elements to the list, we can use the `append!` or `push!` functions. In
Julia, the writing convention is that functions modifying their input have a `!`
at the end. This way, you *know* something is going to happen.

``` julia
julia> push!(numbers, 4)
4-element Array{Int64,1}:
 1
 2
 3
 4
julia> append!(numbers, [5,6,7,8])
8-element Array{Int64,1}:
 1
 2
 3
 4
 5
 6
 7
 8
```

The difference between `push!` and `append!` is that the former adds *single
values*, while the second adds *arrays*. To read the documentation of these
functions, just type `?` at the Julia command prompt, and then type the function name:

``` julia
help?> push!
search: push! pushdisplay

  ..  push!(collection, items...) -> collection

  Insert one or more ``items`` at the end of ``collection``.
```

### Tuples

A tuple is similar to a list in that it's a sequence of elements. However,
tuples can not be changed once created (they are "immutable"). Tuples are
created by placing comma-separated values inside parentheses `()`.

```python
# tuples use paratheses
ATuple= (1,2,3)
anotherTuple = ('blue','green','red')
# notes lists uses square brackets
AList = [1,2,3]
```


### Operators

We can perform mathematical calculations in Python using the basic operators
 `+, -, /, *, %`:

```python
>>> 2 + 2
4
>>> 6 * 7
42
>>> 2 ** 16  # power
65536
>>> 13 % 5  # modulo
3
```

We can also use comparison and logic operators:
`<, >, ==, !=, <=, >=` etc.
`and, or, not`

```python
>>> 3 > 4
False
>>> True and True
True
>>> True or False
True
```

### Challenge
1. What happens when you type `ATuple[2]=5` vs `AList[1]=5` ?
2. Type `type(ATuple)` into python - what is the object type?


## Dictionaries

A **dictionary** is a container that holds pairs of objects - keys and values.

```python
>>> translation = {"one" : 1, "two" : 2}
>>> translation["one"]
1
```
Dictionaries work a lot like lists - except that you index them with *keys*.
You can think about a key as a name for or a unique identifier for a set of values
in the dictionary. Keys can only have particular types - they have to be
"hashable". Strings and numeric types are acceptable, but lists aren't.

```python
>>> rev = {1 : "one", 2 : "two"}
>>> rev[1]
'one'
>>> bad = {[1,2,3] : 3}
...
TypeError: unhashable type: 'list'
```

To add an item to the dictionary we assign a value to a new key:

```python
>>> rev = {1 : "one", 2 : "two"}
>>> rev[3] = "three"
>>> rev
{1: 'one', 2: 'two', 3: 'three'}
```

Using `for` loops with dictionaries is a little more complicated. We can do this
in two ways:

```python
>>> for key, value in rev.items():
...     print(key, "->", value)
...
1 -> one
2 -> two
3 -> three
```

or

```python
>>> for key in rev.keys():
...     print(key, "->", rev[key])
...
1 -> one
2 -> two
3 -> three
>>>
```

## Functions

Defining part of a program in Python as a function is done using the `def`
keyword. For example a function that takes two arguments and returns their sum
can be defined as:

```python
def add_function(a, b):
    result = a + b
    return result

z = add_function(20, 22)
print(z)
42
```

Key points here:

* definition starts with `def`
* function body is indented
* `return` keyword precedes returned value
