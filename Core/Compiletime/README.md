# Compiletime
This document describes the keyword compiletime and its behaviour in eJass language.

# Table of contents

1. [Compiletime](#1-compiletime)
	1. [Compiletime function behaviour](#11-compiletime-function-behaviour)
	2. [Compiletime function arguments](#12-compiletime-function-arguments)
	3. [Compiletime variable](#13-compiletime-variable)
	4. [Compiletime value](#14-compiletime-value)
	5. [Function returning compiletime value](#15-function-returning-compiletime-value)
	6. [Compiletime struct](#16-compiletime-struct)

## 1. Compiletime
Compiletime marks that given block or variable is such, that its value or return value can be deduced during compilation.
Syntax and white space rules for keyword compiletime are described in individual parts where Comp-Qualifier can be used.

### 1.1 Compiletime function behaviour
Compiletime [function](../Function) is such function, that can be called in compiletime environment, such as inside static if block, or from other compiletime function. Additionally, all arguments of such function must also be compiletime values and the return value is also compiletime.

Compiletime function must have all paths in its body evaluable in compiletime. There may only be [if](../Control Flow) or a [loop](../Control Flow) of any sort with compiletime deducible number of iterations, or compiletime deducible exitwhen condition, or [static if](../Control Flow).

Function marked as compiletime can be executed in both compiletime and runtime environment. If compiletime function is never used in runtime environment, the implementation is required to not generate such function in the resulting script.

Function is also considered compiletime if all of its arguments are compiletime and its return type is also marked as compiletime. Static method inside struct can also be declared as compiletime. It may, however, not access any  nonstatic variables of any struct. Non-static method can not be declared as compiletime, instead the whole [struct is marked as compiletime](#16-compiletime-struct).

### 1.2 Compiletime function arguments
Function's arguments can be specified as compiletime explicitly. Such variable can only be initialized to value deducible in compiletime, so return value of compiletime function, or function with compiletime return value, or [literal](../Literals).

### 1.3 Compiletime variable
Compiletime variable is such variable that can have its value deduced in compiletime environment. Both global and local variables can be compiletime. Global compiletime variables can be initialized to literals of [Basic Types](../Type), compiletime [expression](../Basics#6-expression) or to return value of compiletime function or method. Local compiletime variables can be initialized to literals of Basic Types, compiletime expression or to return value of compiletime function or method.

### 1.4 Compiletime value
Compiletime value is any literal to [Basic Type](../Type), compietime expression or compiletime variable.

### 1.5 Function returning compiletime value
Function can mark its return value as compiletime. Such function must return a compiletime deducible value on all paths of execution.

Function with compiletime return value can only be used in in compiletime environment if all of its arguments are marked as compiletime as well, or if the whole function is marked as compiletime, or the function has no arguments..

### 1.6 Compiletime struct
Any [struct](../Struct) can be declared as compiletime, and can then be used in compiletime environment. All members of given struct(methods and variables) are implicitly declared as compiletime as well.