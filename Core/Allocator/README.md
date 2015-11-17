# Allocator
This document covers the allocator [eJass Construct](../Basics#1-ejass-construct).

# Table of contents

1. [Allocator](#1-allocator)
	1. [Syntax](#11-syntax)
		1. [Definition](#111-definition)
			1. [White spaces](#1111-white-spaces)
		2. [Usage](#112-usage)
			1. [White spaces](#1121-white-spaces)

## 1. Allocator
Allocator is special module that can only contain allocate and deallocate methods and variables used inside those functions.

### 1.1 Syntax
#### 1.1.1 Definition
```Jass
opt:(Visibility-Qualifier) allocator allocator_name
	Allocator-Body
endallocator
```

Example:
```Jass
allocator Alloc
	static method allocate takes nothing returns thistype
		return 1
	endmethod
endallocator

private allocator Alloc2
endallocator
```

Allocator can only contain 2 methods with following signature:
* static method allocate takes nothing returns thistype
* method deallocate takes nothing returns nothing

Allocator can additionally contain any number of [variables](../Variable) defined within it. Every defined variable inside allocator must be used in either allocate, deallocate or both. If there is such variable that is not referenced or used inside neither of those, error shall be issued.

#### 1.1.1.1 White spaces
```Jass
opt:(Visibility-Qualifier) allocator allocator_name
	Allocator-Body
endallocator
```

1. Every [Syntactic Unit](../Basics#11-syntactic-unit) is bound tightly to every other Syntactic Unit.
2. endallocator keyword must be placed on separate line, separated from any code.

#### 1.1.2 Usage
```Jass
using opt:(optional) allocator_name
```

Example:
```Jass
allocator A
	...
endallocator

allocator B
	...
endallocator

struct Q
	using A
	using optional B
endstruct
```

[Struct](../Struct) must declare usage of allocator before first method, otherwise it is ignored. If allocator is used, but is not present in the script and keyword optional is not provided, an error is issued.
One struct can declare usage of multiple allocators, and the last existing optional or existing non-optional allocator declared as used will be used.

In the example, the struct Q will use allocator B, if it is found in the script, otherwise it will use A. If A is not found in the script, error will be issued.

#### 1.1.2.1 White spaces
```Jass
using opt:(optional) allocator_name
```

1. Every [Syntactic Unit](../Basics#11-syntactic-unit) is bound tightly to every other Syntactic Unit.
2. The whole line must be placed on separate line, separated from any other code.