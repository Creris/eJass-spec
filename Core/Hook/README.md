# Hook
This document describes the keyword hook and its behaviour in eJass language.

## 1. Hook
Hook allows code to be executed when other code is about to be executed, or right after it finished execution.

### 1.1 Syntax
```Jass
hook opt:(optional) opt:(hookmode) hooked_function hooking_function opt:(priority)
```

Example:
```Jass
hook optional after RemoveUnit onRemove
hook before RemoveUnit onRemoveB priority=54
hook RemoveUnit onRemoveC
```

hooked_function is the function that user wants to hook his function hooking_function to according to hookmode.
Whenever hooked_function is called, the hooking_function will be executed according to the following rules:

	- If hookmode is after, the function will be executed after hooked_function finished its execution.
	- If hookmode is before, the function will be executed before hooked_function is called.

hooking_function can be any static method or function with any Visibility-Qualifier.
hooked_function must be visible to the Scope that the hook is placed inside.
If optional keyword is provided, and hooked_function does not exist, the implementation shall remove such hook request, otherwise error shall be issued.
If no hookmode is provided, before is assumed.
[Priority](../Basics) can be specified for hook.

#### 1.1.1 White spaces
```Jass
hook opt:(optional) opt:(hookmode) hooked_function hooking_function opt:(priority)
```

1. Every Syntactic Unit is bound tightly to every other Syntactic Unit.
2. The whole line must be placed on separate line, separated from any other code.
3. Priority must follow its separate rules, but priority is bound tightly into to_call Callable object.