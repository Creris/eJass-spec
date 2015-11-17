# Auto
This document describes the keyword auto and its meaning in eJass language.

# Table of contents

1. [Auto](#1-auto)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)

## 1. Auto
Keyword auto allows implementation to deduce the type of the [variable](../Variable) from its initialization.
Every variable defined with type auto must be initialized when its defined, otherwise error shall be issued.

### 1.1 Syntax
```Jass
auto variable_name = initialization
```

Example:
```Jass
local auto v = Location(5., 5.)

globals
	private auto st = StringLength("")
endglobals

temporary auto q = CreateGroup()
```

Global, Local and temporary variables can be defined with type auto. The type of variable defined with automatic type is deduced at compilation, and the type is the type that the initialization expression evaluates into.

#### 1.1.1 White spaces
```Jass
auto variable_name = initialization
```

1. Keyword auto is bound tightly into variable_name.
2. Other [Syntactic Units](../Basics#11-syntactic-unit) must follow same rules as with assignment to normal variables(see [Variable](../Variable)).