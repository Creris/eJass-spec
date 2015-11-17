# Enum
This document describes the keyword enum and its behaviour and effects in eJass language.

# Table of contents

1. [Enum](#1-enum)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)

## 1. Enum
Enum defines an enumeration type. Enum can only contain non-mutable variables. Enum can be used as any other type.
Enums provide following methods:
* method toInteger takes nothing returns integer
* method toString takes nothing returns string

The toInteger returns the underlying integer representation for value stored in given enumeration variable.
The toString returns the name in eJass string represetation for value stored in given enumeration variable.

### 1.1 Syntax
```Jass
opt:(Visibility-Qualifier) enum enum_name
	Variable-List
endenum
```

Where Variable-List has following form:
```Jass
variable_name opt:(= compiletime_integer_value)
opt:(, variable_name opt:(= compiletime_integer_value)
opt:(...)
)
```

Example:
```Jass
enum Fruit
	apple = 1,
	banana = 10,
	amount = 2
endenum

private enum PrivEnum
endenum

function f takes Fruit f returns nothing
endfunction

Fruit f = Fruit.apple
call BJDebugMsg(cast<string>(f))	//will print apple
call BJDebugMsg(cast<integer>(f))	//will print 1
```

If variable inside enum does not provide any initial value, the value is calculated as previous + 1. If first variable does not provide value, it equals to 0.
Two variables of different enum types cannot be compared using any of the logical operators. No arithmetic operators can be used on variables of enumeration type.

#### 1.1.1 White spaces
```Jass
opt:(Visibility-Qualifier) enum enum_name
	variable_name opt:(= compiletime_integer_value)
	opt:(, variable_name opt:(= compiletime_integer_value)
	opt:(...)
	)
endenum
```

1. If provided, Visibility-Qualifier is bound tightly into keyword enum.
2. Keyword enum is bound tightly into enum_name
3. enum_name must be followed by finite number of space and tab characters, or [comments](#7-comments) and then new line character.
4. variable_name is bound loosely to preceding [Syntactic Unit](../Basics#11-syntactic-unit).
5. If provided, = is bound tightly into both preceding and following Syntactic Unit.
6. The compiletime_integer_value follows its respective white space rules.
7. endenum must be placed on separate line split from other code.