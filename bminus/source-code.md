
[preprocessor.h](bminus/blob/master/source/preprocessor.h)

- Reads the input line by line, skips comments and deals with the "#line" preprocessor directive.
- Provides a facility to read the next character and peek ahead to the next two characters.

[token.h](bminus/blob/master/source/token.h)

- Splits the input text into identifiable components (tokens): keywords, identifiers, numbers, symbols and literals.
- Provides a facility to read the next token and peek ahead to the next token.
- The implementation is entirely recursive and strictly follows the BNF. One function implements one BNF rule.

[syntax.h](bminus/blob/master/source/syntax.h)

- Takes tokens as input, checks and evaluates program syntax and calls compiler functions to generate code. No intermediate representation is generated.
- The implementation is entirely recursive and strictly follows the BNF. One function implements one BNF rule.

[compiler.h](bminus/blob/master/source/compiler.h)

- Breaks down statements and expressions into simple machine language like sequences.
- Maintains a symbol table for identifiers
- Functions are closely related to the BNF. Rules in syntax.h call one or more dedicated functions in compiler.h

[target_javascript.h](bminus/blob/master/source/target_javascript.h)

[target_cvirtualmachine.h](bminus/blob/master/source/target_cvirtualmachine.h)

[target_linuxassemblerx86.h](bminus/blob/master/source/target_linuxassemblerx86.h)

- Generate target specific output. Mostly just print statements; no clever logic.
- All the above implement the same "interface". Only one may be included in a build.


[bminus.c](bminus/blob/master/source/bminus.c)

 - Initializes all modules and starts the syntax analyzer
 - (the #ifdef and #include statements let Visual Studio compile the code)
 
[errormessages.h](bminus/blob/master/source/errormessages.h) - A simple helper module to print error messages

[stringlib.h](bminus/blob/master/source/stringlib.h) - Generic string functions

[globals.h](bminus/blob/master/source/globals.h) - Declares a couple of constants

