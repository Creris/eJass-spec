# Deprecated
This document describes the keyword deprecated, its behaviour and its effect on callable [eJass Constructs](../Basics#1-ejass-construct] in eJass language.

# Table of contents

1. [Deprecated](#1-deprecated)
	1. [Syntax](#11-syntax)

## 1. Deprecated
Keyword deprecated marks that given callable eJass construct(such as [function](../Function) or [method](../Struct) is deprecated and should no longer be used. If callable eJass construct is marked with deprecated, the compilation will stop with error in Debug mode when such function is called, optionally suggesting user-defined text, or pops warning in Release mode.

### 1.1 Syntax
```Jass
deprecated opt:[(string message opt:(, replacement_function_name))]
```

Example:
```Jass
deprecated("no longer supported") function myF takes nothing
                                               returns integer
    return 5
endfunction

call myF()    //outputs: Error: function myF no longer supported

deprecated function still_Legal_Function takes nothing returns nothing
endfunction

function foo takes integer i returns integer
    return i + 1
endfunction

deprecated("outdated", foo) function bar takes nothing returns boolean
    return false
endfunction

call bar()      //outputs: Error: 
                //function bar outdated.
                //Use function foo takes integer returns integer instead

scope s
    function f takes nothing returns integer
        return 5
    endfunction
endscope

deprecated("", s.f) function q takes nothing returns nothing
endfunction

scope Q
    struct MyStruct
        method a takes nothing returns nothing
        endmethod
		
        static method w takes nothing returns nothing
        endmethod


        deprecated("", thistype.w) method t takes nothing returns nothing
            //Error: deprecated static method MyStruct.t call attempt
            //       Use static method MyStruct.w instead
        endmethod
    endstruct
endscope

deprecated("don't use anymore!", Q.MyStruct.a) function QQ takes nothing returns nothing
    //Error: function QQ don't use anymore!
    //       Use method Q.MyStruct.a instead
endfunction

deprecated("", Q.MyStruct.a) function QW takes nothing returns nothing
    //Error: deprecated function QW call attempt
    //       Use method Q.MyStruct.a instead
endfunction

deprecated("", Q.MyStruct.w) function qqw takes nothing returns nothing
    //Error: deprecated function qqw call attempt
    //       Use static method Q.MyStruct.w instead
endfunction
```

If call to deprecated callable eJass Construct is encountered in debug mode, implementation shall issue an error with impelementation defined error message.
Deprecated can also have a function name as second argument, hinting which function should be called instead. The function name must be valid function name visible to the deprecating function as well as the caller in the script.
Function referenced in deprecated keyword does not need to have the same signature or return type as the deprecated function. If the function is of different signature, its signature is printed in the error/warning message.