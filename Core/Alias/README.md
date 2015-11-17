# Alias
This document covers the alias keyword of the eJass language.

# Table of contents

1. [Alias](#1-alias)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)

## 1. Alias
Alias masks type names so that they can be used as another type name. The aliased type is not different type, so if I alias A to B, where I could pass A, I can pass B too.

### 1.1 Syntax
```Jass
alias existing_type new_type
```

Example:
```Jass
alias integer int
alias SomeLibrary.SomeType T

function f takes int i, int w, T t returns nothing
endfunction
```

Alias can not be used inside [Global Scope](../Basics#41-global-scope). Alias only has effect inside the Scope it is used, including Scopes inside that Scope.

#### 1.1.1 White spaces
```Jass
alias existing_type new_type
```

1. Every [Syntactic Unit](../Basics#11-syntactic-unit) is bound tightly to every other Syntactic Unit.
2. The whole line must be placed on separate line, separated from any other code.