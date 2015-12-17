# Basics
This document covers the fundamental rules of eJass language.

# Table of contents

1. [eJass Construct](#1-ejass-construct)
2. [Keyword](#2-keyword)
	1. [List of keywords](#21-list-of-keywords)
	2. [Context Sensitive](#22-context-sensitive)
3. [Name-referable eJass Construct](#3-name-referable-ejass-construct)
4. [Scope](#4-scope)
	1. [Global Scope](#41-global-scope)
		1. [Modifier Global](#411-modifier-global)
	2. [Scope resolution](#42-scope-resolution)
		1. [Implicit and explicit Scopes](#421-implicit-and-explicit-scopes)
		2. [Scope resolution rules](#422-scope-resolution-rules)
		3. [Keyword using](#423-keyword-using)
			1. [Syntax](#4231-syntax)
				1. [White spaces](#42311-white-spaces)
	3. [Shadowing rules](#43-shadowing-rules)
	4. [Bracket Scoping](#44-bracket-scoping)
5. [Qualifiers](#5-qualifiers)
	1. [Visibility-Qualifier](#51-visibility-qualifier)
	2. [Staticity-Qualifier](#52-staticity-qualifier)
	3. [Readonly-Qualifier](#53-readonly-qualifier)
	4. [Const-Qualifier](#54-const-qualifier)
	5. [Comp-Qualifier](#55-comp-qualifier)
6. [Expression](#6-expression)
	1. [Expression Level](#61-expression-level)
7. [Comments](#7-comments)
	1. [Single line comments](#71-single-line-comments)
		1. [Syntax](#711-syntax)
	2. [Multiple line comments](#72-multiple-line-comments)
		1. [Syntax](#721-syntax)
8. [Constant](#8-constant)
	1. [Constant Function](#81-constant-function)
	2. [Constant arguments](#82-constant-arguments)
	3. [Constant variable](#83-constant-variable)
	4. [Constant value](#84-constant-value)
9. [Optimizations](#9-optimizations)
	1. [Name shortening](#91-name-shortening)
	2. [Function inlining](#92-function-inlining)
	3. [Constant and Compiletime variable inlining](#93-constant-and-compiletime-variable-inlining)
	4. [Expression evaluation](#94-expression-evaluation)
	5. [Dead code removal](#95-dead-code-removal)
	6. [As if rule](#96-as-if-rule)
10. [Operators](#10-operators)
	1. [Arithmetic operators](#101-arithmetic-operators)
	2. [Logical operators](#102-logical-operators)
	3. [Operator precedence](#103-operator-precedence)
11. [Syntactic Unit](#11-syntactic-unit)
12. [White spaces - General](#12-white-spaces---general)
13. [Initialization rules](#13-initialization-rules)
14. [Priorities](#14-priorities)
	1. [Syntax](#141-syntax)
		1. [White spaces](#1411-white-spaces)
15. [Naming Rules](#15-naming-rules)
	1. [Directly inside Scope A](#151-directly-inside-scope-a)
	2. [Rules for Variables](#152-rules-for-variables)
	3. [Rules for Functions and Methods](#153-rules-for-functions-and-methods)
	4. [Rules for library, scope and struct](#154-rules-for-library,-scope-and-struct)
	5. [Rules for other eJass Constructs](#155-rules-for-other-ejass-constructs)
16. [Nesting rules](#16-nesting-rules)
17. [eJass Constructs allowing Visibility-Qualifier](#17-ejass-constructs-allowing-visibility-qualifier)
18. [Most vexing parse](#18-most-vexing-parse)


## 1. eJass Construct
eJass Construct is anything that can be specified in eJass syntax.
All eJass Constructs are case sensitive.

Example:

```Jass
function a takes nothing returns nothing
	...
endfunction

struct A
	...
endstruct
```

## 2. Keyword
Keyword is every sequence of one or more characters that has some syntactical meaning.
Some keywords are [context sensitive](#22-context-sensitive).

### 2.1 List of keywords

|#else           |continue        |endfunction     |final*          |native          |set          
     :----:      |     :----:     |     :----:     |     :----:     |     :----:     |     :----:  
|#elseif         |debug           |endglobals      |for             |needs           |sizeof       
|#endif          |defaults        |endif           |function        |not             |static       
|#if             |defined         |endinterface    |globals         |null            |static_assert
|after*          |deprecated      |endlibrary      |hook            |operator        |struct       
|alias           |destruct        |endloop         |hookable        |optional        |stub         
|allocator       |destructor      |endmethod       |if              |or              |takes        
|and             |else            |endmodule       |implement       |override        |template     
|array           |elseif          |endscope        |import          |priority*       |temporary    
|auto            |encrypted       |endstruct       |initializer     |private         |textmacro    
|before*         |endallocator    |endtextmacro    |inline*         |protected       |then         
|break           |endblock        |endwhile        |interface       |public          |this         
|call            |endconstructor  |enum            |library         |readonly        |thistype     
|cast            |enddestructor   |exitwhen        |library_once    |requires        |true         
|catch           |endenum         |extendor        |local           |return          |type         
|compiletime     |endextendor     |extends         |loop            |returns         |uses         
|constant        |endexternal     |external        |method          |runtextmacro    |using        
|construct       |endexternalblock|externalblock   |module          |scope           |while        
|constructor     |endfor          |false           |				|				 |

### 2.2 Context Sensitive
Every keyword that is context sensitive only have their intended meaning when present in given context.
If these keywords appear outside of their context, they are not treated as keywords but as regular [eJass Constructs](#1-ejass-construct).
List of context sensitive keyword and their respective context:
|Keyword  | Keyword context
  :---:  | :---:
|after    | inside hook expression, inside extendor declaration
|before   | inside hook expression, inside extendor declaration
|final    | inside method declaration
|inline   | inside function or method declaration
|priority | inside hook expression, inside extendor definition

## 3. Name-referable eJass Construct
Name-referable [eJass Construct](#1-ejass-construct) is every eJass construct that can be directly referenced by name.
List of name-referable eJass Constructs:
* [Library](../Library/)
* [Scope](../Scope/)
* [Function](../Function/) and [method](../Struct/)
* [Struct](../Struct/)
* [Extendor](../../Preprocessor/Extendor/)
* [Textmacro](../../Preprocessor/Textmacro/)
* [Module](../../Preprocessor/Module/)
* [Variable](../Variable/)
* [Allocator](../Allocator/)
* [Type](../Type/)
* [Free standing method](../Free Standing Method/)
* [Enum](../Enum/)

## 4. Scope
Scope is every eJass Construct that introduces new logical or other Scope.

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
	Is a Scope that defines given eJass Construct.

3. __External Scope__
	Is every Scope that is not current Scope.

### 4.1 Global Scope
Global Scope is the outer-most Scope of the script. Global Scope is not inside any other Scope and all Scopes are inside Global Scope.
All name-referable eJass Constructs are reachable from Global Scope.

#### 4.1.1 Modifier Global
Special modifier Global can be used as part of the name of name-referable eJass Construct to specify that the [scope resolution](#42-scope-resolution) happens from Global Scope.

### 4.2 Scope resolution
Scope resolution changes relative names of name-referable eJass Constructs into their absolute names including their scopes.
Manual scope resolution is performed using dot syntax.

#### 4.2.1 Implicit and explicit Scopes
There are two types of Scope in eJass:
1. Implicit Scope
	Is such Scope that is directly visible from current Scope and name-referable eJass Constructs inside such Scope can be referenced using their relative name.
		* Global Scope is always Implicit.

2. Explicit Scope
	Is such Scope that is not directly visible from current Scope and name-referable eJass Constructs inside such Scope can not be referenced using relative name.
	They need to use such name that the first Scope in that name is Implicit Scope for current Scope.

#### 4.2.2 Scope resolution rules
Following rules are considered when performing scope resolution:

1. For any given Scope, all its parent Scopes are implicitly declared and considered in Scope resolution and their parts of full path for given eJass construct can be omitted.
2. All other Scopes are External for given point of map script, and to be usable, they must be accessed in such way that the first Scope name in the path is implicitly declared for current Scope.

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

#### 4.2.3 Keyword using
To explicitly declare external Scope as implicit, the keyword using can be used inside any scope to declare that scope as implicit for Scope resolution. Keyword using can only be used on Scopes that are created by scope or library keywords. If using is used and the Scope using is tried to make implicit is not found from that point in script, an error shall be issued.

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
```

1. The whole [expression](#6-Expression) has to be placed on separate line relative to other eJass Constructs.
2. using and scope_name is bound tightly.

#### 4.2.3.1.1 White spaces
```Jass
using scope_name
```

1. keyword using and scope_name are bound tightly into each other

### 4.3 Shadowing rules
If there exists [name-referable eJass construct](#3-name-referable-ejass-construct) called g inside Scope S, and this Scope contains another Scope W which also contains name-referable eJass construct called g, the Scope W will shadow name-referable eJass construct g from beginning of g's definition until the end of scope W. (Case 1)
If there exists name-referable eJass construct called g inside Scope S, and this Scope contains another Scope W which also contains name-referable eJass construct called g, and there exists Scope Q which has both Scope S and Scope W in its list of implicit Scopes, Scope Q will have ambiguous reference to g and error shall be issued. (Case 2)

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

### 4.4 Bracket Scoping
Brackets can also create pseudo scopes for certain [Syntactic Units](#11-syntactic-unit). The lowest bracket-scope is the lowest, and the more opening brackets are encountered, the higher bracket-scope is.

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

## 5. Qualifiers
Qualifiers are [eJass Constructs](#1-ejass-construct) that can be placed before certain other eJass Constructs and they qualify that eJass Construct.

### 5.1 Visibility-Qualifier
Visibility-Qualifier distinguishes whether given eJass Construct is visible to the whole script, or if only the local scope can access and use it. Abbreviation: Vis-Qualifier.

Valid Visibility-Qualifiers:
public(default), private,

### 5.2 Staticity-Qualifier
Staticity-Qualifier determinates whether given eJass Construct is usable only for one instance of struct/interface, or if it is usable by all instances. Abbreviation: Static-Qualifier.

Valid Static-Qualifiers:
static, --none--(default)

### 5.3 Readonly-Qualifier
Readonly-Qualifier determinates whether given eJass Construct is only readable from scopes other than defining scope, or if it is possible to also write to such eJass Construct from external scopes.

Valid Readonly-Qualifiers:
readonly, --none--(default)

### 5.4 Const-Qualifier
Const-Qualifier determinates whether given eJass Construct is constant(unmodifiable), or if it can be modified after definition or declaration. Const-Qualifier before function definition have special meaning.

Valid Const-Qualifiers:
constant, --none--(default)

### 5.5 Comp-Qualifier
Comp-Qualifier determinates whether given eJass Construct can be marked as compiletime, meaning that it can be executed by interpreter during compilation of the map script.

Valid Comp-Qualifiers:
compiletime, --none--(default)

## 6. Expression
Expression is every [eJass Construct](#1-ejass-construct) that expresses some logical executable piece of code.
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

### 6.1 Expression Level
Every expression can be assigned value representing the depth. This value is called level. Top-level expressions are such expressions that are not encapsulated in another expressions. Every expression that is encapsulated in N expressions is of Nth-level expression. Expressions can be any level deep.

## 7. Comments
Comments are [eJass Constructs](#1-ejass-construct) that are ignored when compiling the code.

### 7.1 Single line comments
Single line comments are such comments that only comment certain sequence of characters in given line and the comment ends with next new line symbol.

Example:
```Jass
	nonComment//comment
```

#### 7.1.1 Syntax
```Jass
//comment
```

"//" marks the beginning of single line comment. Single line comments can not be started inside string literals.

### 7.2 Multiple line comments
Multiple line comments are comments that allow user to split their code intelligently.

#### 7.2.1 Syntax
```Jass
not Comment /* comment
/* still
comment */ still comment */ not comment
```

"/*" marks the beginning of multiple line comment.
"*/" marks the ending of multiple line comment.

Multiple line comments can be nested indefinitely. Multiple line comments can never begin or end inside string literals.

## 8. Constant
Constant [eJass Constructs](#1-ejass-construct) are such objects that can not be modified and can not modify state of the runtime environment.

### 8.1 Constant Function
Constant [functions](../Function/) are normal functions with following restrictions:

1. They cannot call other non-constant function
2. They cannot modify the state of runtime environment
3. They cannot modify any of their arguments

### 8.2 Constant arguments
Constant argument is normal function argument with following restrictions:

1. They cannot be modified by the function
2. They may not be passed into function expecting non-constant argument, unless they are of [copyable type](../Type/).

### 8.3 Constant variable
Constant [variable](../Variable) is normal variable with following restrictions:

1. It can only be assigned to at its declaration
2. Rules for Constant argument also apply to constant variables.

### 8.4 Constant value
Constant value is any [literal](../Literals/), [compiletime expression](../Compiletime/) or [constant variable](#83-constant-variable)

## 9. Optimizations
eJass allows following optimizations to be performed on the script:

### 9.1 Name shortening
Every name-referable eJass Construct can have its name shortened to shortest possible length such that it still has unique name.
Special sequences of characters that could be used for name shortening can be reserved by the implementation.

### 9.2 Function inlining
Functions that are marked as inline are requested to be inlined, but the implementation may ignore such requests.
Inlining function is process of logically copying implementation of given function to every place in the script that calls given function.

### 9.3 Constant and Compiletime variable inlining
Every constant and compiletime variable can be inlined into values they are defined with or they evaluate into.

### 9.4 Expression evaluation
If any [expression](#6-Expression) evaluates into value deducible in compilation, the implementation can replace given expression by the value it evaluates into.

### 9.5 Dead code removal
Every [eJass Construct](#1-ejass-construct) and [expression](#6-Expression) that is not referenced anywhere in the script is removed from the script.
Every [comment](#7-comments) is also removed from the script. Every piece of code that is never executed(if false then, or after return value) is removed.

### 9.6 As if rule
The implementation can transform the code in any way if it can guarantee that the runtime environment state after the code is executed will not be different than if the code is not executed. The only allowed runtime environment state change is difference in handle count.

## 10. Operators

### 10.1 Arithmetic operators
Arithmetic operators perform arithmetic operations on operand or operands. List of arithmetic operators:

Id | Op  | Description
1. | +   | Unary: Noop; Binary: Performs arithmetic addition of left and right operands
2. | -   | Unary: Performs arithmetic negation; Binary: Performs arithmetic subtraction of left and right operands
3. | /   | Performs arithmetic division of left and right operands(left divided by right)
4. | *   | Performs arithmetic multiplication of left and right operands
5. | +=  | Performs left = left + right
6. | -=  | Performs left = left - right
7. | /=  | Performs left = left / right
8. | *=  | Performs left = left * right
9. | ++  | Performs left = left + 1
10.| --  | Performs left = left - 1
11.| %   | Performs left - (left/right)*right
12.| %=  | Performs left = left % right
13.| <<  | Performs left * Pow(2, right)
14.| >>  | Performs left / Pow(2, right)
15.| <<= | Performs left = left << right
16.| >>= | Performs left = left >> right

None of operators accept non-implicitly convertible [types](../Type/) as their operands. Operators 9, 10, 11, 12, 13, 14, 15 and 16 only accept integer variables.
If operators 9 or 10 are used on variable, given [expression](#6-expression) must be 1st level expression.
All of these operators can be [overloaded](../Overload Resolution/).

### 10.2 Logical operators
Logical operators perform logical operations on operand or operands. List of logical operators:

Id | Op   | Description
---|------|:----
1. | ==   | Returns true if both sides evaluate to the same boolean
2. | <=   | Returns true if right operand is bigger than or equal to left operand after conversions
3. | >=   | Returns true if right operand is lower than or equal to left operand after conversions
4. | !=   | Returns true if right operand is not equal to left operand after conversions
5. | !    | Negates given boolean operand
6. | not  | Negates given boolean operand
7. | and  | Returns true only if both right and left operands evaluate to true after conversions
8. | or   | Returns true if either right or left operand evaluate to true after conversions

Operators ==, <=, != and >= can accept any implicitly convertible types, and can also accept any mix of integers and reals. Operators 5, 6, 7 and 8 only accept boolean values.
All of these operators can be [overloaded](../Overload Resolution/).

### 10.3 Operator precedence
The following table lists precedence rules for individual operators in order from highest precedence to lowest:

Priority | Name | Example
:----: | -------------------------                   | ----
1.     | Scope resolution                            | X.Y
2.     | Parenthesis                                 | ()
3.     | unary increment/decrement, unary plus/minus | X++, X--, ++X, --X, +X, -X
4.     | bracket operator                            | X[Y]
5.     | not operator                                | not X, !X
6.     | multiplication, division                    | X * Y, X / Y
7.     | addition, subtraction                       | X + Y, X - Y
8.     | bit shift                                   | X << Y, X >> Y
9.     | boolean operators		                     | X < Y, X > Y, X <= Y, X >= Y, X == Y, X != Y
10.    | and, or operators		                     | X and Y, X or Y
11.    | Assignment operator	                     | X = Y, X += Y, X -= Y, X *= Y, X /= Y, X %= Y, X <<= Y, X >>= Y
12.    | Comma operator			                     | X, Y

## 11. Syntactic Unit
Syntactic unit is every non-white space character inside map script, be it name, keyword or parenthesis.

## 12. White spaces - General
All [Syntactic Units](#11-syntactic-unit) can be separated by at least one white space. For example, there must always be space between function and its name, and its name and takes but there needs not to be space between variable and =. If two Syntactical Units can not be separated by new line, they are bound tightly. If two Syntactical Units can be separated by new line, they are bound loosely.

## 13. Initialization rules
Various functions can be marked as initializers which makes them run when the script is initially executed. Initialization goes in this specified order:

1. [Library](../Library/) initializers in following order:
	1. Library's initializer
	2. Initializer of every scope inside such library, which also follow 2.
	3. Initializers of [modules](../../Preprocessor/Module/) implemented inside every struct inside such library
	4. Initializer of every struct inside such library
	5. Free-standing initializer

2. [Scope](../Scope/) initializers in following order:
	1. Scope's initializers
	2. Initializer of every scope inside such scope, which also follow 2.
	3. Initializers of modules implemented inside every struct inside such scope
	4. Initializer of every struct inside such library
	5. Free-standing initializer

3. [Struct](../Struct/) initializers in following order:
	1. Initializers of modules implemented by the struct
	2. Struct's initializer

4. [Free-standing Initializer](../Function/)

## 14. Priorities
Priority defines when should given [eJass Construct](#1-ejass-construct) be executed/evaluated or in other means ran.

### 14.1 Syntax
```Jass
priority=[integer literal]
```

Example:
```Jass
priority = 54
priority = -200
```

If eJass Construct has optional priority, and none is provided, implicit priority of 0 is assumed. If 2 same eJass Constructs are defined with the same priority, the order of evaluation is implementation defined.

#### 14.1.1 White spaces
```Jass
priority = [integer literal]
```

1. priority keyword is bound tightly into following =.
2. = is bound tightly into [integer literal].

## 15. Naming Rules
Every [eJass Construct](#1-ejass-construct) must have a name which does not violate any of the following rules:
* Any character inside the name must be one of: a-z, A-Z, 0-9 or _
* The name must not start with _
* The name must not start with number

### 15.1 Directly inside Scope A
eJass Construct is directly inside [Scope](#4-scope) A if given construct is exactly inside A, not inside any other Scope that is inside A.

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

### 15.2 Rules for Variables
If there exists a [global variable](../Variable/) directly inside Scope A, then there can not be any other global variable directly inside Scope A with the same name, regardless of variable type but can exist in any Scope inside this Scope.

### 15.3 Rules for Functions and Methods
There can be 2 [functions](../Function/) or [methods](../Struct/) directly inside Scope A, if they have different number or types of parameters, one of them is template and one is template specialization, both are template specializations or they are both templates with different number of template arguments or they are non-template with different number of arguments.

### 15.4 Rules for library, scope and struct
If there is [library](../Library/), [scope](../Scope/) or [struct](../Struct/) directly inside Scope A, then there can not be another library, scope or struct directly inside Scope A with the same name.

### 15.5 Rules for other eJass Constructs
For any given eJass Construct there directly inside Scope A there can not exist another eJass Construct of same type with the same name directly inside Scope A.

## 16. Nesting rules
Some [eJass Constructs](#1-ejass-construct) can be nested into each other.
The following table defines which eJass Construct can contain other eJass Constructs, and which:

         Name         								| Can be inside
	:----------		  								|     :---------:
[library](../Library/)               				| Global Scope
[scope](../Scope/)                 					| Global Scope, library
[function](../Function/)              				| Global Scope, library, scope
[struct](../Struct/)               					| Global Scope, library, scope, struct
[module](../../Preprocessor/Module/)  			    | Global Scope, library, scope
[method](../Struct/)                				| struct, module
[globals block](../Variable/)         				| Global Scope, library, scope
[interface](../Interface/)            		 		| Global Scope, library, scope
[concept](../Concept/)               				| Global Scope, library, scope
[import](../../Preprocessor/Import/)        		| Global Scope, library, scope
[free standing method](../Free Standing Method/)  	| Global Scope, library, scope
[allocator](../Allocator/)             				| Global Scope, library, scope
[hook](../Hook/)                  					| Global Scope, library, scope
[textmacro](../../Preprocessor/Textmacro/)          | Global Scope, library, scope
[extendor](../../Preprocessor/Extendor/)            | Global Scope, library, scope
[enum](../Enum/)                  					| Global Scope, library, scope
[alias](../Alias/)                 					| library, scope
[static_assert](../Static Assert/)         			| anywhere
[loop](../Control Flow/)                  			| any callable, any control block
[while](../Control Flow/)                 			| any callable, any control block
[for](../Control Flow/)                   			| any callable, any control block
[if block](../Control Flow/)              			| any callable, any control block
[variable declaration](../Variable/)  				| globals block, any callable, struct, any control block, module, allocator
[external](../../Preprocessor/External/)            | Global Scope, library, scope, struct
[externalblock](../../Preprocessor/External/)       | Global Scope, library, scope, struct
textmacro execute         							| anywhere
extendor execute          							| anywhere
[static if](../Control Flow/)             			| anywhere except external and externalblock

## 17. eJass Constructs allowing Visibility-Qualifier
Following [eJass constructs](#1-ejass-construct) allow Visibility-Qualifiers to be used inside their Scope:
* [library](../Library/)
* [scope](../Scope/)
* [struct](../Struct/)
* [module](../../Preprocessor/Module/)
* [globals block](../Variable/)
* [allocator](../Allocator/)

## 18. Most vexing parse
The implementation is required to parse the input script in such way that if two tokens are after each other in such way that they can form different lexeme as compared to when parsed separated, they will always form the lexeme consisting of both tokens.
Exception to this rule is >>, which will be parsed as >> only if the preceding list did not contain template instantiation.