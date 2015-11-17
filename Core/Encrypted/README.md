# Encrypted
This document describes the keyword encrypted and its effects on [strings](../Type) in eJass language.

# Table of contents

1. [Encrypted](#1-encrypted)
	1. [Syntax](#11-syntax)
		1. [White spaces](#111-white-spaces)

## 1. Encrypted
Keyword encrypted allows the implementation to encrypt defined strings when using raw string data with certain algorithms. Decryption happens if every attempt to use the string. Implementations can optimize this decryption by decrypting the string once and storing that inside local variable and then using that decrypted string instead of decrypting on every use.
Note: while encryption has no performance impact, the decryption requires time linearly scaling with size of string, and may eat a lot of OPs, resulting in Op-heavy code crashing the running virtual thread because of decryption of strings. Use this wisely.

### 1.1 Syntax
```Jass
local encrypted opt:[(Enc_Mode)] string opt:(array) string_name
opt:(= Initial_value)
```

Example:
```Jass
local encrypted string s = "My awesome" + " brutally encrypted string!"
local encrypted(ME) string s = "Even better encryption!"
```

String encryption only happens on raw strings, and happens at compile time, resulting in no performance impact of the code. Decryption must be done during runtime, resulting in possibly huge impact on Ops as a result of decryption.

If encrypted does not provide Enc_Mode, a default Enc_mode is used from eComp.
Possible Enc_Mode-s:

Name | Description
:----|-----
Nan  | No encryption
No   | No encryption
None | No encryption
BAE  | Requires the implementation to modify every character inside the string such that its ASCII value is modified
BSE  | Requires the implementation to modify every character inside the string such that the position may be changed
ME   | Requires the implementation to modify every character inside the string such that the ASCII value is modified as well as position can be changed

#### 1.1.1 White spaces
```Jass
local encrypted opt:[(Enc_Mode)] string opt:(array) string_name
opt:(= Initial_value)
```

1. encrypted is bound to local, as well as following [Syntactic Unit](../Basics#11-syntactic-unit) tightly.
2. (Enc_Mode), if provided, is bound to both preceding and following Syntactic Unit tightly.
3. The remaining Syntactic Units follow white spacing rules for [variables](../Variable).