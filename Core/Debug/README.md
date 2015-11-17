# Debug
This document describes keyword debug, its behaviour and debug mode in eJass language.

# Table of contents

1. [Debug Mode](#1-debug-mode)
2. [Keyword debug](#2-keyword-debug)

## 1. Debug Mode
Debug mode can be represented by global variable DEBUG_MODE(set to true when compiling in debug mode) or with keyword debug.
The implementation is required to define 2 compiletime global boolean variables usable anywhere in the script called DEBUG_MODE and RELEASE_MODE. When compiling in debug mode, DEBUG_MODE is set to true and RELEASE_MODE is set to false. When compiling in release mode, RELEASE_MODE is set to true and DEBUG_MODE is set to false.

## 2. Keyword debug
When any Syntactic Unit is preceded with debug keyword, given [Syntactic Unit](../Basics#11-syntactic-unit) will only be generated when the script is being compiled in Debug mode.

Example:
```Jass
debug function f takes nothing returns string
    debug local string s = "Some String literal"
    debug return s
debug endfunction

function w takes nothing returns nothing
    local string toPrint = "TPr"
    debug if SomeFunction() then
        debug call BJDebugMsg(Compiler.nameOf(SomeFunction) + 
                              " returned true! RUN AWAY!")
    debug else
        call BJDebugMsg(toPrint)
    debug endif
endfunction
```

[Function](../Function) f as well as its contents will only be generated when the script is being compiled in debug mode.
The whole [if](../Control Flow) structure, excluding the Print of toPrint inside the Else-Block will only be compiled if the script is being compiled in debug mode.