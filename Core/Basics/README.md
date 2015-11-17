# Basics
This document covers the fundamental rules of eJass.

# 1. [eJass Construct](#1-eJass-Construct)
[eJass Construct](#1-eJass-Construct) is anything that can be specified in eJass syntax.
All [eJass Construct](#1-eJass-Construct)s are case sensitive.

Example:

```Jass
function a takes nothing returns nothing
	...
endfunction

struct A
	...
endstruct
```

# 2. Keyword
Keyword is every sequence of one or more characters that has some syntactical meaning.
Some keywords are [context sensitive](#22-Context-Sensitive).

## 2.1 List of keywords

\#else           |constructor     |endfor          |false           |native          |set             
     :----:     |     :----:     |     :----:     |     :----:     |     :----:     |     :----:     
\#elseif         |continue        |endfunction     |final*          |needs           |sizeof          
\#endif          |debug           |endglobals      |for             |not             |static          
\#if             |defaults        |endif           |function        |null            |static_assert   
after*          |defined         |endinterface    |globals         |operator        |struct          
alias           |deprecated      |endlibrary      |hook            |optional        |stub            
allocator       |destruct        |endloop         |hookable        |or              |takes           
and             |destructor      |endmethod       |if              |override        |template        
array           |else            |endmodule       |implement       |priority        |temporary       
auto            |elseif          |endscope        |import          |private         |textmacro       
before*         |encrypted       |endstruct       |initializer     |protected       |then            
break           |endallocator    |endtextmacro    |inline*         |public          |this            
call            |endblock        |endwhile        |interface       |readonly        |thistype        
cast            |endconstructor  |exitwhen        |library         |requires        |true            
catch           |enddestructor   |extendor        |local           |return          |type            
compiletime     |endextendor     |extends         |loop            |returns         |uses            
constant        |endexternal     |external        |method          |runtextmacro    |using           
construct       |endexternalblock|externalblock   |module          |scope           |while           

## 2.2 Context Sensitive
Every keyword that is context sensitive only have their intended meaning when present in given context.
If these keywords appear outside of their context, they are not treated as keywords but as regular [eJass Constructs](#1-eJass-Construct).
List of context sensitive keyword and their respective context:
Keyword | Keyword context
| :---: | :---:
after   | inside hook expression, inside extendor declaration
before  | inside hook expression, inside extendor declaration
final   | inside method declaration
inline  | inside function or method declaration

# 3. Name-referable [eJass Construct](#1-eJass-Construct)
Name-referable [eJass Construct](#1-eJass-Construct) is every eJass construct that can be directly referenced by name.
List of name-referable [eJass Construct](#1-eJass-Construct)s:
* [Library]()
* [Scope]()
* [Function and method]()
* [Struct]()
* [Extendor]()
* [Textmacro]()
* [Module]()
* [Variable]()
* [Allocator]()
* [Type]()

# 4. Scope
Scope is every [eJass Construct](#1-eJass-Construct) that introduces new logical or other scope.

Example:
```Jass
library L
	//Scope L
	scope Q
		//Scope L.Q
		function f takes nothing returns nothing
			//Scope L.Q.f
			if true then
				//Scope L.Q.f.if_1
			endif
		endfunction
		//Scope L.Q
	endscope
	//Scope L
endlibrary
//Scope Global
function a takes nothing returns nothing
	//Scope a(same as Scope Global.a)
endfunction
```

Special Scopes:

1. __Global Scope__
	Is the top-most Scope.
  
2. __Defining Scope__
	Is a Scope that defines given [eJass Construct](#1-eJass-Construct).

3. __External Scope__
	Is every Scope that is not current Scope.

## 4.1 Global Scope
Global Scope is the outer-most Scope of the script. Global Scope is not inside any other Scope and all Scopes are inside Global Scope.
All name-referable [eJass Construct](#1-eJass-Construct)s are reachable from Global Scope.

### 4.1.1 Modifier Global
Special modifier Global can be used as part of the name of name-referable [eJass Construct](#1-eJass-Construct) to specify that the [scope resolution](#42-Scope-resolution) happens from Global Scope.

## 4.2 Scope resolution
Scope resolution changes relative names of name-referable [eJass Construct](#1-eJass-Construct)s into their absolute names including their scopes.
Scope resolution is performed using dot syntax.

### 4.2.1 Implicit and explicit Scopes
There are two types of Scope in eJass:
1. Implicit Scope
	Is such Scope that is directly visible from current Scope and name-referable [eJass Construct](#1-eJass-Construct)s inside such Scope can be referenced using their relative name.
		* Global Scope is always Implicit.

2. Explicit Scope
	Is such Scope that is not directly visible from current Scope and name-referable [eJass Construct](#1-eJass-Construct)s inside such Scope can not be referenced using relative name.
	They need to use such name that the first Scope in that name is Implicit Scope for current Scope.

### 4.2.2 Scope resolution rules
Following rules are considered when performing scope resolution:

1. For any given Scope, all its parent Scopes are implicitly declared and considered in Scope resolution and their parts of full path for given eJass construct can be omitted.
2. All other Scopes are External for given point of map script, and to be usable, they must be accessed by their full path.

Example:
```Jass
library A
    function f takes nothing returns nothing
    endfunction
	
    scope S
        function q takes nothing returns nothing
            call f()            //same as call A.f() or Global.A.f()
        endfunction
	endscope
	
	function w takes nothing returns nothing
        call q()                //compilation error: q is undeclared from 
                                //this scope
        call S.q()              //fine, will call A.S.q
    endfunction
endlibrary
```

### 4.2.3 Keyword using
To explicitly declare External Scope as implicit, the keyword using can be used inside any scope to declare that scope as implicit for Scope resolution. Keyword using can only be used on Scopes that are created by scope or library keywords. If using is used and the Scope using is tried to make implicit is not found from that point in script, an error shall be issued.

Example:
```Jass
library A
    using S        //compilation error: No scope S found
    
    scope S
        function f takes nothing returns nothing
        endfunction
        
        scope Q
            function w takes nothing returns nothing
            endfunction
        endscope
    endscope
    
    using S
    using S.Q
    
    function q takes nothing returns nothing
        call f()
        call w()
    endfunction
endlibrary
```

#### 4.2.3.1 Syntax
```Jass
using scope_name

1. The whole [expression](#6-Expression) has to be placed on separate line relative to other [eJass Construct](#1-eJass-Construct)s.
2. using and scope_name is bound tightly.
```
#### 4.2.3.1.1 White spaces
```Jass
using scope_name
```

## 4.3 Shadowing rules
If there exists name-referable eJass construct called g inside Scope S, and this Scope contains another Scope W which also contains name-referable eJass construct called g, the Scope W will shadow name-referable eJass construct g from beginning of g's definition until the end of scope W. (Case 1)
If there exists name-referable eJass construct called g inside Scope S, and this Scope contains another Scope W which also contains name-referable eJass construct called g, and there exists Scope Q which has both Scope S and Scope W in its list of implicit Scopes, Scope Q will have ambiguous reference to g and error will be issued. (Case 2)

Example:
```Jass
scope S
    function f takes nothing returns nothing
    endfunction
    
    scope Q
        function f takes nothing returns nothing
            call f()        //will call S.Q.f
                            //Case 1
        endfunction
        
        function w takes nothing returns nothing
            call f()        //will call S.Q.f
                            //Case 1
        endfunction
    endscope
    
    function w takes nothing returns nothing
        call f()            //will call S.f
                            //Case 1
    endfunction
endscope

scope W
    using S
    using Q        //same as using S.Q at this point
    
    //Error: function f() defined multiple times
    

    function r takes nothing returns nothing
        call f()            //Error: Could not resolve call to f()
                            //Could be: S.f, S.Q.f
                            //Case 2
    endfunction
    
    function f takes nothing returns nothing
                            //Error: function f() defined multiple times
    endfunction
endscope
```

## 4.4 Bracket Scoping
Brackets can also create pseudo scopes for certain [Syntactic Units](#11-Syntactic-Unit). The lowest bracket-scope is the lowest, and the more opening brackets are encountered, the higher bracket-scope is.

Example:
```Jass
A(B((C(D)E)F)G)Q
```

A is in bracket-scope 0
B is in bracket-scope 1
C is in bracket-scope 3
D is in bracket-scope 4
E is in bracket-scope 3
F is in bracket-scope 2
G is in bracket-scope 1
Q is in bracket-scope 0

# 5. Qualifiers
Qualifiers are [eJass Construct](#1-eJass-Construct)s that can be placed before certain other [eJass Construct](#1-eJass-Construct)s and they qualify that [eJass Construct](#1-eJass-Construct).

## 5.1 Visibility-Qualifier
Visibility-Qualifier distinguishes whether given [eJass Construct](#1-eJass-Construct) is visible to the whole script, or if only the local scope can access and use it. Abbreviation: Vis-Qualifier.

Valid Visibility-Qualifiers:
public, private, --none--(defaults public)

## 5.2 Staticity-Qualifier
Staticity-Qualifier determinates whether given [eJass Construct](#1-eJass-Construct) is usable only for one instance of struct/interface, or if it is usable by all instances. Abbreviation: Static-Qualifier.

Valid Static-Qualifiers:
static, --none--

## 5.3 Readonly-Qualifier
Readonly-Qualifier determinates whether given [eJass Construct](#1-eJass-Construct) is only readable from scopes other than defining scope, or if it is possible to also write to such [eJass Construct](#1-eJass-Construct) from external scopes.

Valid Readonly-Qualifiers:
readonly, --none--

## 5.4 Const-Qualifier
Const-Qualifier determinates whether given [eJass Construct](#1-eJass-Construct) is constant(unmodifiable), or if it can be modified after definition or declaration. Const-Qualifier before function definition have special meaning.

Valid Const-Qualifiers:
constant, --none--

## 5.5 Comp-Qualifier
Comp-Qualifier determinates whether given [eJass Construct](#1-eJass-Construct) can be marked as compiletime, meaning that it can be executed by interpreter during compilation of the map script.

Valid Comp-Qualifiers:
compiletime, --none--

# 6. Expression
Expression is every [eJass Construct](#1-eJass-Construct) that expresses some logical executable piece of code.
Assignment, function call, control flow block and return statements are expressions.
Expressions are always evaluated in left to right order.

Example:
```Jass
call A(B(), C(D()), E())
```

will evaluate in following order:
1. call to B()
2. call to D()
3. call to C()
4. call to E()
5. call to A()

## 6.1 Expression Level
Every expression can be assigned value representing the depth. This value is called level. Top-level expressions are such expressions that are not encapsulated in another expressions. Every expression that is encapsulated in N expressions is of Nth-level expression. Expressions can be any level deep.

# 7. Comments
Comments are [eJass Construct](#1-eJass-Construct) that are ignored when compiling the code.

## 7.1 Single line comments
Single line comments are such comments that only comment certain sequence of characters in given line and the comment ends with next new line symbol.

Example:
```Jass
	nonComment//comment
```

### 7.1.1 Syntax
```Jass
//comment
```

"//" marks the beginning of single line comment. Single line comments can not be started inside string literals.

## 7.2 Multiple line comments
Multiple line comments are comments that allow user to split their code intelligently.

### 7.2.1 Syntax
```Jass
not Comment /* comment
/* still
comment */ still comment */ not comment
```

Multiple line comments can be nested indefinitely. Multiple line comments can never begin or end inside string literals.

# 8. Constant
Constant [eJass Construct](#1-eJass-Construct)s are such objects that can not be modified and can not modify state of the runtime environment.

## 8.1 Constant Function
Constant [functions]() are normal functions with following restrictions:

1. They cannot call other non-constant function
2. They cannot modify the state of runtime environment
3. They cannot modify any of their arguments

## 8.2 Constant arguments
Constant argument is normal function argument with following restrictions:

1. They cannot be modified by the function
2. They may not be passed into function expecting non-constant argument, unless they are of [copyable type]().

## 8.3 Constant variable
Constant [variable]() is normal variable with following restrictions:

1. It can only be assigned to at its declaration
2. Rules for Constant argument also apply to constant variables.

## 8.4 Constant value
Constant value is any [literal](), [compiletime expression]() or [constant variable](#83-Constant-variable)

# 9. Optimizations
eJass allows following optimizations to be performed on the script:

## 9.1 Name shortening
Every name-referable [eJass Construct](#1-eJass-Construct) can have its name shortened to shortest possible length such that it still has unique name.
Special sequences of characters that could be used for name shortening can be reserved by the implementation.

## 9.2 Function [inlining]()
Functions that are marked as inline are requested to be inlined, but the implementation may ignore such requests.
Inlining function is process of logically copying implementation of given function to every place in the script that calls given function.

## 9.3 Constant and Compiletime variable inlining
Every constant and compiletime variable can be inlined into values they are defined with or they evaluate into.

## 9.4 Expression evaluation
If any [expression](#6-Expression) returns value deducible in compilation, the implementation can replace given expression by the value it returns.

## 9.5 Dead code removal
Every [eJass Construct](#1-eJass-Construct) and [expression](#6-Expression) that is not referenced anywhere in the script is removed from the script.
Every [comment](#7-Comments) is also removed from the script. Every piece of code that is never executed(if false then, or after return value) is removed.

## 9.6 As if rule
The implementation can transform the code in any way if it can guarantee that the runtime environment state after the code is executed will not be different than if the code is not executed.

# 10. Operators

## 10.1 Arithmetic operators
Arithmetic operators perform arithmetic operations on operand or operands. List of arithmetic operators:
1. +
2. -
3. /
4. *
5. +=
6. -=
7. /=
8. *=
9. ++
10. --
11. % 
12. %=
13. <<
14. >>
15. <<=
16. >>=

None of operators accept non-implicitly convertible types as their operands. Operators 9, 10, 11, 12, 13, 14, 15 and 16 only accept integer variables.

## 10.2 Logical operators
Logical operators perform logical operations on operand or operands. List of logical operators:
1. ==
2. <=
3. >=
4. !=
5. !
6. not
7. and
8. or

Operators ==, <=, != and >= can accept any implicitly convertible types, and can also accept any mix of integers and reals. Operators 5, 6, 7 and 8 only accept boolean values(unless overloaded).

## 10.3 Operator precedence
The following table lists precedence rules for individual operators in order from highest precedence to lowest:

Priority | Name | Example
:----: | ------------------------- | ----
1.     | Parenthesis               | ()
2.     | unary increment/decrement | X++, X--, ++X, --X
3.     | bracket operator          | X[Y]
4.     | dot operator              | X.Y
5.     | not operator              | not X, !X
6.     | multiplication, division  | X * Y, X / Y
7.     | addition, subtraction     | X + Y, X - Y
8.     | bit shift                 | X << Y, X >> Y
9.     | boolean operators		   | X < Y, X > Y, X <= Y, X >= Y, X == Y, X != Y
10.    | and, or operators		   | X and Y, X or Y
11.    | Assignment operator	   | X = Y, X += Y, X -= Y, X *= Y, X /= Y, X %= Y, X <<= Y, X >>= Y
12.    | Comma operator			   | X, Y

# 11. Syntactic Unit
Syntactic unit is every non-white space character inside map script, be it name, keyword or parenthesis.

# 12. White spaces - General
Some [Syntactic Units](#11-Syntactic-Unit) have to be separated by at least one white space. For example, there must always be space between function and its name, and its name and takes but there needs not to be space between variable and =. If two Syntactical Units can not be separated by new line, they are bound tightly. If two Syntactical Units can be separated by new line, they are bound loosely.

# 13. Initialization rules
Various functions can be marked as initializers which makes them run when the script is initially executed. Initialization goes in this specified order:

1. [Library]() initializers in following order:
	1. Library's initializer
	2. Initializer of every scope inside such library, which also follow 2.
	3. Initializers of modules implemented inside every struct inside such library
	4. Initializer of every struct inside such library
	5. Free-standing initializer

2. [Scope]() initializers in following order:
	1. Scope's initializers
	2. Initializer of every scope inside such scope, which also follow 2.
	3. Initializers of modules implemented inside every struct inside such scope
	4. Initializer of every struct inside such library
	5. Free-standing initializer

3. [Struct]() initializers in following order:
	1. Initializers of modules implemented by the struct
	2. Struct's initializer

4. [Free-standing Initializer]()

# 14. Priorities
Priority defines when should given [eJass Construct](#1-eJass-Construct) be executed/evaluated or in other means ran.

## 14.1 Syntax
```Jass
priority=[integer literal]
```

Example:
```Jass
priority = 54
priority = -200
```

If [eJass Construct](#1-eJass-Construct) has optional priority, and none is provided, implicit priority of 0 is assumed. If 2 same [eJass Constructs](#1-eJass-Construct) are defined with the same priority, the order of evaluation is implementation defined.

### 14.1.1 White spaces
```Jass
priority = [integer literal]
```

1. priority keyword is bound tightly into following =.
2. = is bound tightly into [integer literal].

# 15. Naming Rules

## 15.1 Directly inside [Scope]() A
eJass Construct is directly inside Scope A if given construct is exactly inside A, not inside any other Scope that is inside A.

Example:
```Jass
scope A
    function f takes nothing returns nothing
    endfunction	
    scope B
        function w takes nothing returns nothing
        endfunction
    endscope
endscope
```

f is directly inside Scope A, while w is not directly inside Scope A, but is directly inside Scope B.

## 15.2 Rules for [Variables]()
If there exists a [global]() variable directly inside Scope A, then there can not be any other global variable directly inside Scope A with the same name, regardless of variable type but can exist in any Scope inside this Scope.

## 15.3 Rules for [Functions]() and [Methods]()
There can be 2 functions or methods directly inside Scope A, if they have different number or types of parameters, one of them is template and one is template specialization, both are template specializations or they are both templates with different number of template arguments or different number of arguments.

## 15.4 Rules for [library](), [scope]() and [struct]()
If there is scope introducer directly inside Scope A, then there can not be another scope introducer directly inside Scope A with the same name.

## 15.5 Rules for other eJass Constructs
For any given eJass Construct there directly inside Scope A there can not exist another eJass Construct of same type with the same name directly inside Scope A.

# 16. Nesting rules
Some eJass Constructs can be nested into each other.
The following table defines which eJass Construct can contain other eJass Constructs, and which:

         Name         | Can be inside
	:----------		  |     :---------:
library               | Global Scope
scope                 | Global Scope, library
function              | Global Scope, library, scope
struct                | Global Scope, library, scope, struct
module                | Global Scope, library, scope
method                | struct, module
globals block         | Global Scope, library, scope
interface             | Global Scope, library, scope
concept               | Global Scope, library, scope
import                | Global Scope, library, scope
free standing method  | Global Scope, library, scope
allocator             | Global Scope, library, scope
hook                  | Global Scope, library, scope
textmacro             | Global Scope, library, scope
extendor              | Global Scope, library, scope
enum                  | Global Scope, library, scope
alias                 | library, scope
static_assert         | anywhere
loop                  | any callable, any control block
while                 | any callable, any control block
for                   | any callable, any control block
if block              | any callable, any control block
variable declaration  | globals block, any callable, struct, any control block, module, allocator
external              | Global Scope, library, scope, struct
externalblock         | Global Scope, library, scope, struct
textmacro execute     | anywhere
extendor execute      | anywhere
static if             | anywhere except external and externalblock

# 17. eJass Constructs allowing Visibility-Qualifier
Following eJass constructs allow Visibility-Qualifiers to be used inside their Scope:
* library
* scope
* struct
* module
* globals block
* allocator





























