# Interface
This document describes the behaviour of interfaces as well as the keyword interface in eJass language.

# Table of contents

1. [Interface](#1-interface)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)
		2. [Method declaration](#112-method-declaration)
			1. [White spaces](#1121-white-spaces)

## 1. Interface
Interfaces provide a way to define a type that cannot be instanced, but provides a set of methods that all child of given interface shall implement.

### 1.1 Syntax
```Jass
opt:(Visibility-Qualifier) interface name
	Interface-Body
endinterface
```

Example:
```Jass
private interface inter
	method m takes nothing returns nothing
	method q takes integer i, integer w returns nothing
	method e takes nothing returns integer defaults (0)
endinterface
```

Interface cannot extend any type, nor can it extend any other interface.
Any type that extends interface must provide definition for all methods declared in given interface, otherwise this type is considered only a partial specialization of given interface and is not instance-able. Any attempt to create instance of partially specialized type shall result in error.

#### 1.1.1 White spaces
```Jass
opt:(Visibility-Qualifier) interface name
	Interface-Body
endinterface
```

1. keyword interface is bound tightly into preceding Visiblity-Qualifier, if any, as well as the name of the interface.
2. Interface-Body must be separated by at least one new line character at both beginning and end
3. endinterface keyword must be separated from anything else.

#### 1.1.2 Method declaration
```Jass
opt:(Visibility-Qualifier) opt:(Comp-Qualifier) opt:(Const-Qualifier)
opt:(Staticity-Qualifier) method opt:(operator) name
takes opt:(arguments) returns return_type opt:(defaults (impl_value))
```

Example:
```Jass
method m takes integer i, integer a returns real
method q takes integer i, integer c returns integer defaults (i * c + 5)
```

Arguments are of the same form as arguments to any function or method.

Keywords deprecated and inline cannot be used in method declaration, and if they are provided, error shall be issued.
Interface can only contain declarations of non-static methods. Visibility-Qualifier and stub keywords are ignored, if provided in the declaration.
Methods declared inside interface can be marked with keyword defaults followed by a compiletime expression that evaluates to same type as the return type of that method inside parenthesis. This expression can contain any literal value as well as compiletime values and calls.
The expression can also contain non-compiletime expression that is dependant on the arguments of that function, for basic types.
Methods declared in interface can also have implicit values. These values can not be changed for overriding methods from child types.

##### 1.1.2.1 White spaces
```Jass
opt:(Visibility-Qualifier) opt:(Comp-Qualifier) opt:(Const-Qualifier)
opt:(Staticity-Qualifier) method opt:(operator) name
takes opt:(arguments) returns return_type opt:(defaults (impl_value))
```
1. Comp-Qualifier and Const-Qualifier are bound tightly into each other if provided, and bound loosely to preceding and following Syntactic Unit.
2. Staticity-Qualifier is bound loosely to both preceding and following Syntactic Unit.
3. operator keyword, if provided is bound tightly into method keyword.
4. Preceding keyword is bound tightly to the name of method, which is bound tightly to the takes keyword.
5. all arguments are bound loosely to each other with [special rules](../Function).
6. returns keyword is bound loosely to the last argument specified in argument's list or to nothing keyword otherwise.
7. Return type with its Qualifiers is bound loosely to returns keyword.
8. If Const-Qualifier is specified for return type, it is bound tightly with the return type.
9. Keyword defaults, if provided, is bound tightly into preceeding Syntactic Unit.
10. impl_value is bound tightly into defaults and every sub-expression inside impl_value follows its own rules for white spaceing.
11. Every other Syntactic Unit is bound tightly to each other.