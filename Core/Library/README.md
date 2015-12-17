# Library
This document describes the behaviour as well as keyword library in eJass language.

# Table of contents

1. [Library](#1-library)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)

# 1. Library
Library is eJass construct that guarantees encapsulation and introduces new scope.
Libraries can contain other eJass constructs excluding another library. Libraries themselves can only be placed inside Global Scope or in namespace.

## 1.1 Syntax
```Jass
library lib_name opt:(initializer init_func_name) opt:(requires/uses/needs opt:[optional] required_lib_name...)
    Library-Body
endlibrary
```

Required libraries syntax:
```Jass
opt:(requires) lib_name1 opt:(, opt:[requires] lib_name2, ...)
```

When library wants to use code from another library, it needs to specify that it uses that library. Using eJass constructs from library that the using library does not specify shall issue an error.
Library can require any number of other libraries and if more than one is required, they shall be split with comma.
If required library is marked as optional and is not present in the script, the request to use that library is ignored, otherwise error shall be issued.
Library can provide initializer, which is single function or static method that is found inside that library. Scoping for that function starts at the library level.
Any code inside Global Scope(such as scope) outside of another library may use code from any library.
Any eJass construct that is identified by name and is not marked as private that is defined inside library and is used must include the library name as part of the scope.

Example:
```Jass
library L
	function f takes nothing returns nothing
	endfunction
endlibrary

function w takes nothing returns nothing
	call L.f()
endfunction
```

### 1.1.1 White spaces
```Jass
library lib_name opt:(initializer init_func_name) opt:(requires/uses/needs opt:[optional] required_lib_name...)
    Library-Body
endlibrary
```
```Jass
opt:(requires) lib_name1 opt:(, opt:[requires] lib_name2, ...)
```

1. library keyword is bound tightly into lib_name.
2. Lib_name is bound tightly into initializer keyword, if provided.
3. initializer keyword is bound tightly into init_func_name, if provided.
4. If needs, uses or requires keyword is provided, it is bound tightly into the previous Syntactic Unit.
5. If needs, uses or requires is provided, it is bound tightly into the first required library entry.
6. optional keyword is bound tightly into the following lib_name.
7. Required_library_name is bound tightly with the following comma, if any more required libraries are provided.
8. Required library entries are bound loosely into each other.
9. Every other Syntactic Unit is bound tightly into each other.