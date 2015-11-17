# Function
This document describes functions, their syntax and their behaviour in eJass language.

# Table of contents

1. [Function](#1-function)
	1. [Function Signature](#11-function-signature)
	2. [Syntax](#12-syntax)
		1. [Definition](#121-definition)
			1. [Function argument](#1211-function-argument)
			2. [White spaces](#1212-white-spaces)
			3. [Argument](#1213-argument)
		2. [Calling](#122-calling)
			1. [White spaces](#1221-white-spaces)
	3. [Function Overloading](#13-function-overloading)
	4. [Anonymous functions](#14-anonymous-functions)
		1. [Syntax](#141-syntax)
			1. [White spaces](#1411-white-spaces)
	5. [Initialization Function](#15-initialization-function)

## 1. Function
Function is a callable, addressable eJass construct, which can have arguments passed into it and it can also return value. Functions encapsulate some common code that is executed when the function is called.

### 1.1 Function Signature
Function's signature consists of function's name and its number and type of arguments and return type.

### 1.2 Syntax
#### 1.2.1 Definition
```Jass
opt:(deprecated) opt:(inline) opt:(Visibility-Qualifier)
opt:(Comp-Qualifier) opt:(Const-Qualifier)
function function_name takes opt:(arguments) returns
opt:(Comp-Qualifier) opt:(Const-Qualifier) return_type_name
    ...
endfunction
```

Arguments are of following form:

```Jass
opt:(Comp-Qualifier) opt:(Const-Qualifier) type_name1 variable_name1
opt:(, opt:(Comp-Qualifier) opt:(Const-Qualifier) type_name2 variable_name2 ...)
```

Example:
```Jass
deprecated function myF takes nothing returns nothing
    ...
endfunction

inline function myF takes integer a, integer b returns compiletime string
    return "5"
endfunction

deprecated inline function myF takes integer a returns constant integer
    return a
endfunction

constant function q takes nothing returns compiletime string
    compiletime local string s = "gg"
    return s
endfunction

constant function qW takes nothing returns compiletime string
    return "qq"
endfunction

function qWW takes nothing returns compiletime string
    return qW()
endfunction

function www takes compiletime constant integer i returns nothing
endfunction
```

Every argument may declare required type as constant and/or [compiletime](../Compiletime), and arguments must be comma separated.
If function takes no arguments, then "nothing" shall be wrote between takes and returns. If function takes arguments, then they must be fully defined and they must be separated by commas.
If function does not return anything, "nothing" must be used as return type. If function does return some type, then all returned values of given function must be implicitly convertible into specified return type, but they don't need to be of same type. Return type may me marked with constant, in which case the caller may only store it inside constant variable, or pass into other function as constant argument, or the variable must be of copyable type. Return type can be marked with [compiletime](../Compiletime#15-function-returning-compiletime-value).

#### 1.2.1.1 Function argument's implicit values
Function arguments may have implicit values specified, meaning that when the function is called, some arguments may end up unspecified from callers point of view, and have implicit value defined inside the function definition.

```Jass
opt:(Comp-Qualifier) opt:(Const-Qualifier) type_name1 variable_name1 opt:(= Initial Value) opt:(, ...)
```

Example:
```Jass
... takes compiletime constant string s, constant integer i,
          unit u = null returns ...
```

Implicit value of variable N may only be specified, if all function's arguments to the right from N already have implicit value specified. Implicit value can also be return value of function, in which case, return value of function must be implicitly convertible to the type of variable setting implicit value for, or must be cast explicitly to given type.

#### 1.2.1.2 White spaces
```Jass
opt:(deprecated) opt:(inline) opt:(Visibility-Qualifier)
opt:(Comp-Qualifier) opt:(Const-Qualifier)
function function_name takes opt:(arguments) returns
opt:(Comp-Qualifier) opt:(Const-Qualifier) return_type_name
    ...
endfunction
```

1. If both deprecated and inline keywords are specified, they are bound tightly.
2. If Visibility-Qualifier is specified, and inline keyword is specified, they are bound tightly.
3. If Comp-Qualifier is specified, it is bound loosely to preceding Syntactic Unit.
4. If Const-Qualifier keyword is specified, it is bound tightly to both preceding Syntactic Unit and 'function' keyword.
5. function keyword is bound tightly to the name of function, which is bound tightly to the 'takes' keyword.
6. all arguments are bound loosely to each other with special rules.
7. returns keyword is bound tightly to the last argument specified in argument's list or to nothing keyword otherwise.
8. Return type with its Qualifiers is bound tightly to 'returns' keyword.
9. If Const-Qualifier and/or Comp-Qualifier are specified for return type, they are bound tightly with the return type and each other.
10. Every other Syntactic Unit is bound tightly to each other.

#### 1.2.1.3 Argument's White spaces
```Jass
opt:(Comp-Qualifier) opt:(Const-Qualifier) type_name1 variable_name1 opt:(= Initial Value) opt:(, ...)
```

1. If Const-Qualifier or Comp-Qualifier are specified, they are bound tightly to name of argument's type and to each other.
2. Argument's name is bound tightly to name of argument's type.
3. Comma following argument N, if specified is bound tightly into argument's name.
4. If argument has default value, the = as well as the expression initializing the argument are bound tightly to each other as well as argument's name and following comma.
5. Every independent argument is bound loosely to each other.

### 1.2.2 Calling
```Jass
call function_name(opt:(arguments))
```

Arguments are of following form:
```Jass
opt:(value1) opt:(, value2 opt:(, value3 ...))
```

Example:
```Jass
call myF(1)
call myW(1, 2, null)
call myQ()
call myZ(
         1,
         2,
         3,
         null
        )
```
             
Every argument can be enclosed in any number of parenthesis. However, every comma must be inside the top bracket-scope.

Example:
```Jass
call myF(((1)), (2))    //valid
call myF(((1), 2))      //invalid, comma before second argument is
                        //not in top bracket-scope
```

#### 1.2.2.1 White spaces
```Jass
call function_name(opt:(arguments))
```

1. Keyword call and function's name are bound tightly.
2. Opening parenthesis is bound tightly with function's name.
3. If no arguments are provided, closing parenthesis is bound tightly with opening parenthesis.
4. If any number of arguments are provided, then the first argument is bound loosely to the opening parenthesis, the last argument is loosely bound to the last parenthesis and arguments are bound loosely between each other.
5. Every other Syntactic Unit is bound tightly to each other.

Argument's white spaces:
```Jass
opt:(value1) opt:(, value2 opt:(, value3 ...))
```

1. If multiple arguments are provided, then the value is bound tightly to following comma, and following comma is bound loosely to the next value.

## 1.3 Function Overloading
Functions may have the same name, and only be differentiated by their signature(see 4.1 - Function's Signature). If two functions only differentiate in signature by types that are implicitly convertible, these functions are conflicting.

Example:
```Jass
constant function myF takes integer i returns boolean
    return i > 5
endfunction

function myF takes unit u returns integer
    return GetUnitLifePercent(u)
endfunction

call myF(GetUnitLifePercent(null))    //calls myF(integer)
call myF(null)                        //calls myF(unit)

function myF2 takes unit u returns nothing
function myF2 takes location l returns nothing

call myF2(Location(0., 0.))       //fine, calls myF2(Location)
call myF2(null)                   //compiler error: ambiguous myF2(null),
                                  //can be myF2(unit) or myF2(location)

call myF2(cast<unit>(null))       //fine, calls myF2(unit)
```

### 1.4 Anonymous functions
Anonymous functions are functions that have no name but have the code they contain defined.

#### 1.4.1 Syntax
```Jass
opt:(Comp-Qualifier) opt:(Const-Qualifier) function(opt:(arguments)) opt:(return_type) opt:(catch) {
    //body
}
```

Example:
```Jass
constant function(integer i) boolean catch{
    return i > externalVariable
}
```

Constness as well as arguments must follow normal function rules. If function does not return anything, no return type is required. Catch keyword allows anonymous function to use variables local to Defining Scope. If anonymous function is not constant and catches Defining Scope's variables, it may modify them.

#### 1.4.1.1 White spaces
```Jass
opt:(Comp-Qualifier) opt:(Const-Qualifier) function(opt:(arguments)) opt:(return_type) opt:(catch) {
    //body
}
```

1. [Comp-Qualifier](../Basics#5-qualifiers) and Const-Qualifier are bound tightly into each other as well as with function keyword, when provided.
2. function keyword is bound tightly to the opening parenthesis.
3. In case of no arguments, closing parenthesis is bound tightly to opening parenthesis.
4. For every argument of type T and name N, T and N are bound tightly, together with following comma, if another argument exists.
5. Return type, if specified, is bound loosely to the closing parenthesis of arguments.
6. catch keyword, if specified is bound tightly with return type, if specified.
7. Opening curly brackets is bound loosely with its preceding Syntactic Unit, be it catch, return type or closing parenthesis.
8. Closing curly bracket must be placed on separate line in regards to the body of anonymous function.
9. If explicit call is specified after closing curly bracket, the same rules apply as to normal [function's call](#122-calling).
10. Every other Syntactic Unit is bound tightly to each other.

### 1.5 Initialization Function
Initialization functions are such functions that perform some initial initialization of code. Initialization functions must have specific name, otherwise they act as normal functions.
Name format: InitTrig_XXXX.
Where XXXX can be any sequence of characters such that the function will still have valid name.