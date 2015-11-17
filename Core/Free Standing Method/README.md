# Free Standing Method
This document describes the behaviour and syntax of free standing methods in eJass language.

# Table of contents

1. [Free Standing Method](#1-free-standing-method)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)

## 1. Free Standing Method
Free standing method provides a way for the script to extend [build-in types](../Type) with [struct](../Struct)-like syntax.

### 1.1 Syntax
```Jass
opt:(deprecated) opt:(inline) opt:(Visibility-Qualifier) opt:(Comp-Qualifier)
opt:(Const-Qualifier) method Type.method_name takes opt:(arguments)
returns opt:(Comp-Qualifier) opt:(Const-Qualifier) return_type
    Free-Standing_Method-Body
endmethod
```

Example:
```Jass
private method integer.increment takes nothing returns nothing
    set this++
endmethod
```

this variable always stores the object for running instance. Free standing methods may not be declared inside structs.
Rules other than mentioned above as well as white spacing of Free standing methods are the same as member [methods](../Struct).
If Free standing method is declared as private, it will only be usable inside that Scope.
Free standing method can not be declared as static, cannot be declared as stub or override and cannot be operator.

### 1.1.1 White spaces
```Jass
opt:(deprecated) opt:(inline) opt:(Visibility-Qualifier) opt:(Comp-Qualifier)
opt:(Const-Qualifier) method Type.method_name takes opt:(arguments)
returns opt:(Comp-Qualifier) opt:(Const-Qualifier) return_type
    Free-Standing_Method-Body
endmethod
```

1. Free-standing methods follow the exact same rules of white spacing as normal methods do, but Type, "." and method_name are all bound to each other tightly.