# Conversion Operator
This document describes the conversion operator in eJass language.

# Table of contents

1. [Conversion Operator](#1-conversion-operator)
	1. [Invocation syntax](#11-invocation-syntax)
		1. [White spaces](#111-white-spaces)
	2. [Predefined conversion operators](#12-predefined-conversion-operators)

## 1. Conversion Operator
Conversion operator allows the script to explicitly cast one type to another type, if possible.

### 1.1 Invocation syntax
```Jass
cast<Type>(expression)
```

Conversion operator will attempt to convert the type that the expression evaluates into into another type Type provided as template parameter into keyword cast. If expression cannot be converted to Type or to type that is implicitly convertible into Type, error shall be issued.
Conversion operator is always explicit, therefore the type to convert to must be fully mentioned at all times.

#### 1.1.1 White spaces
```Jass
cast<Type>(expression)
```

1. The cast invocation follows the same syntax as call to templated function when providing the template arguments explicitly.

### 1.2 Predefined conversion operators
The implementation shall provide following list of conversion operators:
1. cast<string>(integer)
2. cast<string>(real)
3. cast<integer>(string)
4. cast<real>(string)
5. cast<integer>(real)
6. cast<real>(integer)
7. cast<integer>(user_type)
8. cast<user_type>(integer)
9. cast<boolean>(user_type)

If user-defined conversion operator is provided for integer, 7 will call that operator instead of doing direct conversion from integer to instance.
If user-defined [constructor](../Struct) taking exactly one integer is provided, 8 will call that constructor and return new instance of user_type instead of doing direct conversion.
If user-defined conversion operator is provided for boolean, 9 will call that operator instead of doing direct conversion from integer to instance.