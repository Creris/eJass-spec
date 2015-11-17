# Control Flow
This document describes rules and behaviour of various control flow constructs in eJass language.

# Table of contents

1. [Control Flow](#1-control-flow)
	1. [Conditional block](#11-conditional-block)
		1. [Syntax](#111-syntax)
			1. [White spaces](#1111-white-spaces)
	2. [Compiletime conditional block](#12-compiletime-conditional-block)
		1. [Syntax](#121-syntax)
			1. [White spaces](#1211-white-spaces)
	3. [Control flow modifiers](#13-control-flow-modifiers)
		1. [exitwhen](#131-exitwhen)
			1. [White spaces](#1311-white-spaces)
		2. [break](#132-break)
			1. [White spaces](#1321-white-spaces)
		3. [continue](#133-continue)
			1. [White spaces](#1331-white-spaces)
	4. [Loop](#14-loop)
		1. [Syntax](#141-syntax)
			1. [White spaces](#1411-white-spaces)
	5. [While loop](#15-while-loop)
		1. [Syntax](#151-syntax)
			1. [White spaces](#1511-white-spaces)
	6. [For loop](#16-for-loop)
		1. [Syntax](#161-syntax)
			1. [White spaces](#1611-white-spaces)

## 1. Control Flow
Control flow allows user to control how the code runs in various ways.

### 1.1 Conditional block
Conditional block allows user run code only if certain condition is met.

#### 1.1.1 Syntax
```Jass
if condition opt:(bool_operand condition2 opt:(...)) then
    ...
elseif boolean_evaluated_expression then
    ...
else
    ...
endif
```

Example:
```Jass
if myBooleanVar and not GetBoolean(null, 5.42 * 1.22) then
    call MyFunction1()
elseif not myBooleanVar then
    call MyOtherFunction()
else
    call SomeYetAnotherFunction()
endif

if A or (not B and C) and D then
    ...
endif
```

Condition is such [Syntactic Unit](../Basics#11-syntactic-unit), that is implicitly evaluable into [boolean](../Type). Condition can be [variable](../Variable), return value of [function](../Function) or validly enclosed [bracket-scope](../Basics#44-bracket-scoping). not keyword is part of condition.

Example:
```Jass
if not A and B then
```

* not A is first condition
* B is second condition

If-Block is block of code from if keyword up until then keyword.
Then-Block is block of code after then keyword up until matching else, endif or elseif keyword.
ElIf-Block is block of code after then keyword preceded by elseif keyword up until matching else, endif or elseif keyword.
Else-Block is block of code after else keyword up until matching endif keyword.

Keyword if must always be followed by valid condition, that is implicitly convertible into boolean.
Additionally, the condition may be preceded by not boolean operator, which will negate the resulting boolean condition. If there is condition A, then the next boolean operator after that must be in the same bracket-scope(see 1.3.1 – Bracket-scope), or in lower bracket-scope then A itself, or A must not be in 0th bracket-scope.
Not boolean operator is always tight up into the following condition. And and, or boolean operators must be separated by exactly one condition.
If one of the operands of and boolean operator is evaluated to false, then the whole and operator is evaluated to false. If one of the operands of or boolean operator is evaluated to true, then the whole or operator is evaluated to true.
Evaluation of conditions always goes from right to left.

If the result of initial if-block is true, the body of then-block is executed. Otherwise, the next if-block begins evaluation, if there is any, or the else-block starts execution, if it exists. Any number of if blocks can be nested into any block.

#### 1.1.1.1 White spaces
```Jass
if condition opt:(bool_operand condition2 opt:(...)) then
    ...
elseif boolean_evaluated_expression then
    ...
else
    ...
endif
```

1. if keyword is bound tightly to the following condition.
2. then keyword is bound tightly to the preceding condition.
3. elseif, else and endif keywords must be placed on separate lines.
4. Everything else is bound loosely.

### 1.2 Compiletime conditional block
[Compiletime](../Compiletime) conditional block controls code generation during compilation.
Special values are usable, and reserved, when using compiletime conditional block:
* DEBUG_MODE
* RELEASE_MODE = not DEBUG_MODE

DEBUG_MODE evaluates to true, when script is being compiled in debug mode, and to false otherwise.

#### 1.2.1 Syntax
```Jass
#if condition opt:bool_operand opt:(opt:not_operand value...) then
    //SThen-Block
#elseif ... then
    //ElIf-Block
#else
    //Else-Block
#endif
```

Every other [eJass Construct](../Basics#11-syntactic-unit) can be found within compiletime conditional block, including other compiletime conditional blocks. Compiletime conditional block can be found within any other eJass construct, excluding external block.
Any variable declared within compiletime conditional block is not defined before the condition of given conditional block is evaluated.
Example:
```Jass
globals
    #if SomeExternalGlobalDeclaredBelow then
        //evaluates to false
        integer a = 4
    #endif
    
    #if defined(a) then
        //since the above static if evaluated to false,
        //the integer never got created so this evaluates
        //to false just as much
        integer b
    #endif
endglobals
```

[Variable](../Variable) a will not be created, because SomeExternalGlobalDeclaredBelow was not defined at that point, so it evaluates to false, and defined(a) evaluates into false too, since a was not included from the previous block, so variable b will not be defined either.

The whole condition must be compiletime. If any sub-expression of the whole condition is not evaluable in compiletime, the whole condition is evaluated to false. Example is trying to read non compiletime variables, which will instantly yield false for the whole condition.

#### 1.2.1.1 White spaces
```Jass
#if condition opt:bool_operand opt:(opt:not_operand value...) then
    //SThen-Block
#elseif ... then
    //ElIf-Block
#else
    //Else-Block
#endif
```

1. #if keyword and the first condition are bound tightly.
2. then keyword and the last condition are bound tightly.
3. Values are bound loosely into each other.
4. Every and and or boolean operator is bound tightly to the following value.
5. not keyword that is part of value is bound tightly into that value.
6. #if, #elseif, #else and #endif keywords must be placed on separate lines.
7. Every other Syntactic Unit is bound tightly to each other.

### 1.3 Control flow modifiers
#### 1.3.1 exitwhen
Exitwhen is special keyword followed by expression, that is evaluable into boolean result.
If the boolean result evaluates into true, the control flow structure is immediately stopped and the code execution continues after the end of given control flow structure. Exitwhen can only appear inside [loop](#14-loop).

#### 1.3.1.1 White spaces
```Jass
exitwhen boolean_expression
```

1. Exitwhen and boolean_expression are bound tightly.
2. Boolean_expression must follow its respective rules for white spacing.
3. The whole expression must be placed on separate line from any other code.

#### 1.3.2 break
When break is hit inside control flow structure, the control flow structure immediately ends its execution and lets the code after it run. Break is equal to exitwhen true. Break can only appear inside Loop, While and For.

#### 1.3.2.1 White spaces
```Jass
break
```

1. The Break expression must be placed on separate line from any other code.

#### 1.3.3 continue
When continue is hit inside control flow structure, the code immediately jumps back into the beginning of given control flow structure. Continue can only appear inside Loop, While and For.

#### 1.3.3.1 White spaces
```Jass
continue
```

1. The Continue expression must be placed on separate line from any other code.

### 1.4 Loop
Loop is control flow structure, that allows certain code to run multiple times, according to some condition.

#### 1.4.1 Syntax
```Jass
loop
    opt:(expressions)
endloop
```

Example:
```Jass
loop
    call A(myInteger)
    set i = i + 1
    
    if (myInteger == 5) then
        call B()
        set myInteger++
        continue
    endif
    
    exitwhen myInteger > 10
    set myInteger += 1
endloop
```

Loop-Body is block of code within loop and endloop keywords.
Exit-Cond is condition after exitwhen keyword.
Break-Expr is expression of type break.
Continue-Expr is expression of type continue.
Loop-Body can be filled with any number of valid expressions, including 0, as well as with 3 special expressions: exitwhen, break keywords and continue.
The Loop-Body will be executed until any of the encountered Exit-Cond evaluate to true, or until Break-Expr is hit, or until the OP limit is reached.
Exit-Cond, Break-Expr and Continue-Expr are bound into the closest higher loop block.
One loop block can have any number of Exit-Cond, Break-Expr and Continue-Expr, and they can be nested into other eJass constructs, such as if block.

#### 1.4.1.1 White spaces
```Jass
loop
    opt:(expressions)
endloop
```

1. loop keyword, as well as endloop keyword must be placed on separate lines from any other code.
2. Every expression must follow rules of corresponding white spacing rules.
3. Exit-Cond as well as Break-Expr expressions must be placed on separate lines.
4. Condition after exitwhen keyword must follow the same rules as conditions inside [If-Block](#11-conditional-block) and additionally the first expression of the condition is bound tightly into exitwhen keyword. Special case is, when the first expression is parenthesis pair, which must begin on the same line as the preceding exitwhen.

### 1.5 While loop
While loop is control flow structure that runs certain code for as long as given condition evaluates into true.

#### 1.5.1 Syntax
```Jass
while (condition)
    opt:(expressions)
endwhile
```

Example:
```Jass
while (myInteger <= 10)
    call A(myInteger)
    set i = i + 1
    set myInteger += 1
endwhile
```

While-Body is block of code between while and endwhile keywords.
While-Cond is condition following while keyword.
While-Body gets executed as long as While-Cond evaluates to true.
While-Cond must evaluate, or be implicitly convertible into boolean.
Result of While-Cond is checked on all iterations, including the first, before entering While-Body and gets checked after every iteration before entering While-Body on next iteration.

#### 1.5.1.1 White spaces
```Jass
while (condition)
    opt:(expressions)
endwhile
```

1. while as well as endwhile keywords must be placed on separate lines from any other code.
2. All expressions inside While-Body must follow corresponding white spacing rules, so for expression of type function call, the rules listed in 4.2.2.1 - White spaces.
3. Condition after while keyword must follow the same rules as conditions inside If-Block(5.1.1 - Syntax) and additionally the first expression of the condition is bound tightly into opening parenthesis.
4. while keyword is bound tightly into opening parenthesis.
5. Every other Syntactic Unit is bound tightly into each other.

### 1.6 For loop
For loop is a control flow structure that allows certain code to run multiple times, depending on some condition.

#### 1.6.1 Syntax
```Jass
for (opt:[initializer_list]; opt:[condition]; opt:[stepper_list])
    opt:(expressions)
endfor
```

Example:
```Jass
for(integer i = 0, integer j = someMaxValue; i < j; ++i)
    call uberFunction(myEvenMoreUberArray[i])
endfor
```

For-Init is code block from opening parenthesis until first semicolon.
For-Cond is code block from first semicolon until second semicolon.
For-Step is code block from second semicolon until the closing parenthesis.
For-Body is code between the closing parenthesis and endfor keyword.

For-Init, For-Cond as well as For-Step are optional, and can be left empty for no code in that part specified.

For-Init runs only once, before the first execution of For-Body.
For-Cond runs before every iteration inside For-Body, including after For-Init.
For-Step runs after every iteration inside For-Body.

Iteration over For-Body stops only when For-Cond evaluates to true.

Initializer_list serves to initialize variables used inside for body, and therefore variables defined inside Initializer_list cannot be referenced by name from outside the For-Body, For-Cond, For-Step or For-Init.
Initializer_list can not have free standing function calls or set operations.
Variables defined inside Initializer_list are shadowing any other variables with the same name. Variables defined inside Initializer_lsit must be separated by comma from each other.
All Variables defined inside Initializer_list must have their type specified, omitting local or temporary keyword. All Variables defined inside Initializer_list are local, and can not be declared as temporary.

Condition must be evaluable, or implicitly convertible into boolean value.

Stepper_list servers to advance state of variables. Stepper_list can perform any number of calls of any function or set operations, but can not define variables.

All three For blocks(For-Init, For-Cond and For-Step) are optional, and they can be provided in any combination. The semicolons, are however not optional, and must be provided even with any, or all empty blocks.

#### 1.6.1.1 White spaces
```Jass
for (opt:[initializer_list]; opt:[condition]; opt:[stepper_list])
    opt:(expressions)
endfor
```

1. for keyword is bound tightly into opening parenthesis.
2. opening parenthesis is bound to 'local' keyword, if any variables are provided, otherwise the semicolon following initializer_list is bound tightly to opening parenthesis.
3. All variables declared inside initializer_list must be separated by comma, and are bound loosely to each other. However, comma after every variable defined, but the last one, is bound tightly to the variable defined before it.
4. Variable definition must follow the same rules as normal variable definition(3.2 Local Variable)
5. Condition is bound loosely to the previous semicolon.
6. Condition is bound tightly to the following semicolon.
7. If no condition is provided, semicolons are bound loosely to each other.
8. First expression of Stepper_list is bound tightly to previous semicolon.
9. If no Stepper_list is provided, the closing parenthesis is bound loosely into second semicolon.
10. Stepper_list expressions must be separated with commas, and are bound loosely to each other.
11. Expressions inside Stepper_list must follow their respective rules, including white spacing.
12. Every other Syntactic Unit is bound tightly into each other.