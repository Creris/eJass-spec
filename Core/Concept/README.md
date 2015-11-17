# Concept
This document describes the keyword concept and the [eJass Construct](../Basics#1-ejass-construct) tied to it in eJass language.

# Table of contents

1. [Constraint](#1-constraint)
2. [Concept](#2-concept)
	1. [Syntax](#21-syntax)
		1. [White spaces](#211-white-spaces)

## 1. Constraint
[Templates](../Template) depending on constraints are only instantiated when all constraints evaluate to true. One template can have any number of comma separated constraints. Constraints can be either [Concept](#2-concept), or a compiletime condition, or a special [operator call](../Type)

Example:
```Jass
template <type T> requires Arithmetic<T>, T != Table,
                           T extends MyAwesomeType and
                           !(T extends MyAnotherAwesomeType),
                           Compiler.isParentOf(T, SomeType) and
                           Compiler.isSame(T, T)
function f takes T t returns T
    return t
endfunction
```

## 2. Concept
Concepts are a constraint feature for [templates](../Template), providing compiletime information about [types](../Type). Concepts can only be used as constraints for templates or other concepts. Concepts evaluate into true if and only if all methods or functions declared inside given concept exist for given type and when all concepts that this concept depends on also evaluate to true. Otherwise, they evaluate into false.

### 2.1 Syntax
```Jass
opt:(Visibility-Qualifier) concept name
	Concept-Body
endconcept
```

Example:
```Jass
template <type T>
concept Arithmetic
    method T.operator+ takes T rhs returns T
    method T.operator- takes T rhs returns T
    method T.operator* takes T rhs returns T
    method T.operator/ takes T rhs returns T
    //constructor takes T
    //destructor takes T
    //static method T.met_hod takes T something returns nothing
    //stub method T.m ...
    //function add takes T a, T b returns T
endconcept

concept Another requires integer != real,
                         TypeInfo.isChildOf(unit, handle)
    method MyStruct.myMethod takes nothing returns nothing
endconcept


concept General requires Another
    function SomeFunction takes integer something returns integer
endconcept
```

Concepts can be templated, and can put following requirements on any type visible to the concept:
1. existence of method with given name, arguments and return type
2. existence of function with given name, arguments and return type

Concept can only become constraint after it has been fully defined. Concept itself can have constraints. If function or method with given name, signature and return type does not exist, the concept returns false.

#### 2.1.1 White spaces
```Jass
opt:(Visibility-Qualifier) concept name
	Concept-Body
endconcept
```

1. Visibility-Qualifier, if provided, is bound to keyword concept loosely.
2. concept and Name are bound tightly.
3. Concept-Body must start at separate line.
4. endconcept must be placed at separate line.