# Inheritance and polymorphism
This document describes the polymorphism and inheritance models in eJass language.

# Table of contents

1. [Inheritance](#1-inheritance)
	1. [Syntax](#11-syntax)
	2. [Order of construction and destruction of types](#12-order-of-construction-and-destruction-of-types)
2. [Polymorphism](#2-polymorphism)
	1. [Ad-hoc polymorphism](#21-ad-hoc-polymorphism)
	2. [Parametric polymorphism](#22-parametric-polymorphism)
	3. [Inclusion polymorphism](#23-inclusion-polymorphism)

## 1. Inheritance
In eJass, inheritance defines a parent-child relationship between types.
When a type inherits from another type, it will inherit all of its public and protected methods and variables excluding its constructor and destructor.
When members of parent that are marked as public are inherited, they are visible from outside the child. When members of parent that are marked as protected are inherited, they are not visible outside of the child.
Constructor of child type will always call constructor of parent type and destructor of child type will always call destructor of parent type.
One child can have multiple parents.

If one child type A inherits from parents B and C that inherit from type D, child A will only have one copy of D inherited.
The type A will only call the constructor of D once inside of its constructor, and call D's destructor only once inside its destructor.

Types that are not defined by user using the [struct syntax](../Struct) can not be inherited.

When function or method expects its parameter to be of any type, any child type of that type can be passed into it instead.
Only methods defined inside the expected type can be used in such place.

### 1.1 Syntax
```Jass
struct Child extends Parent1, Parent2, ...
```

### 1.2 Order of construction and destruction of types
All structs are constructed starting at top-most parent and ending at the struct itself, meaning that the constructor of child struct will call constructor of its parents before executing itself, who will call constructors of their parents before executing themselves and so on, until types with no parents are reached.
All structs are destructed starting at bottom-most type and ending at the top-most parent, meaning that the destructor of child struct will call destructor of its parents after executing itself, who will call destructors of their parents after executing themselves and so on, until types with no parents are reached.
Types are always constructed in order of declaration, and destructed in reversed order.

Example:
```Jass
struct A extends B, C
```

When constructing instance of A, it will first call constructor of B, then call constructor of C and afterwards will finish constructing itself.
When destructing instance of A, it will first run its body, then call destructor of C and finally destructor of B.

## 2. Polymorphism
In eJass, there are 3 types of polymorphism: Ad-hoc polymorphism, parametric polymorphism and inclusion polymorphism.

### 2.1 Ad-hoc polymorphism
See [Function overloading](../Function#13-function-overloading).

### 2.2 Parametric polymorphism
See [Templates](../Template).

### 2.3 Inclusion polymorphism
Types are called polymorphic when at least one of its methods is defined as stub. This type defines that it provides some functionality that can be overriden by its child types.
If reference of type A is expected, but type B is given instead, and call to stub method is requested, the implementation is required to execute call to that method inside type B, unless type B does not override given method of A, in which case the method inside A shall be executed instead.