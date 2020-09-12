

The syntax of B-minus is a strict subset of C. Given the same program an ANSI C compiler should produce the same results.


Built in functions
------------------

**fgetc()** and **fputc()** provide single character i/o with the operating system

**exit()** terminates the process

**debug()** is only used in regression tests to print a value

```c
fgetc ( stdin )
fputc ( <expression> , stdout)
fputc ( <expression> , stderr)
exit  ( <expression> )
debug ( <expression> )
```

fgetc(), fputc(), and exit() are equivalent to the matching C library calls.

When generating assembler the built in functions are implemented using Linux system calls.

main()
------

No arguments. Value of return statement ignored. Use exit() to terminate the process with an exit code.

```c
main() {
   ...
   exit(0);
}
```

<!-- more -->

Constants
---------

Constants are declared using the enum keyword. 

```
enum { enumerator-list } ;
```

Example:
```c
enum {A, B C};  // A=0, B=1, C=2
enum {D=1,E}; // D=1, E=2
```
Only constants may be declared, enum types and enum variables are not supported.

String literals
---------------

String literals are only allowed in function calls as a parameter. Each character in the string is one char (32 bit) wide.

```c
say("Hello\n");
```

Escape sequences are: \n \r \t \\ \' \"

Character constants
-------------------

```
c = 'a' + 1;
```

Escape sequences are: \n \r \t \\ \' \"

Data types
----------

Primitive types are int and char, they are synonymous, both are 32 bit integers. Cannot be initialised in the declaration.

```c
int identifier;
char identifier;
```

Arrays are declared with a constant size specifier. One dimension only.

```c
int array_identifier[10];

enum {Length=10};
char array_identifier[Length];
```

Pointers may be declared only in function definitions as a function parameter. Only the array syntax is supported. No pointer arithmetic may be used.

```c
strlength(char str[]) {
   ...
}
```

Variables do not have a defined initial value.

Functions
---------

Functions are declared without a return type. The default return type is "int".

```c
divide(int dividend, int divisor) {
   return dividend / divisor;
}
```

Functions may be called before being declared.

Comments
--------

Comments are C++ style

```c
// this is a comment
```

Global and local variables
--------------------------

Usual rules apply, local variables hide global variables with the same name. No ":" operator to explicitly access global variables.

Assignment statement
--------------------

Assignments are statements, not expressions as in C.

```c
i = i + 1; // OK
func(i = i + 1); // not OK
```

if statement
------------

Same syntax and rules as in C

```c
if (toolow()) gohigher(); else stay();
```

while statement
---------------

Same syntax and rules as in C, except no **break** or **continue** statements

```c
while (toolow()) goabithigher();
```

return statement
----------------

Same syntax and rules as in C. Cannot be used to set process exit code.


Operators
---------

**logical**:
logical not, logical or, logical and, equal, not equal, less than, less than or equal, greater than, greater than or equal

**arithmetic**:
plus, minus, multiply, divide, unary plus, unary minus

Operator precedence same as in C



Preprocessor
------------

The only preprocessor directive recognised is: **#line**

All other preprocessor directives are ignored.

**#line** is always followed by the number 2 and a file name in double quotes, no spaces allowed in the file name. (this allows #line to be wrappped in an #ifdef directive)

```c

#line 2 "program.c"
```

BNF grammar
-----------

```
token
            integer-constant
            character-constant
            string-literal
            identifier-or-keyword
            symbol

integer-constant
            digit-list

character-constant
            ' char-escape-sequence '
            ' any-except ' '
           
digit-list
            digit digit-list
            digit

digit
            one-of 0 1 2 3 4 5 6 7 8 9
           
string-literal
            " string-literal-char-sequence "

string-literal-char-sequence
            string-literal-char string-literal-char-sequence
            string-literal-char

string-literal-char
            char-escape-sequence
            any-char-except "

char-escape-sequence
            one-of \n \r \t \\ \' \"

identifier-or-keyword
            identifier-nondigit-char identifier-any-char-sequence
            identifier-nondigit-char
           
identifier-nondigit-char
            one-of _ a b c d e f g h i j k l m n o p q r s t u v w x y z
                     A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
           
identifier-any-char
            one-of _ a b c d e f g h i j k l m n o p q r s t u v w x y z
                     A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
                     0 1 2 3 4 5 6 7 8 9
          
identifier-any-char-sequence
            identifier-any-char identifier-any-char-sequence
            identifier-any-char

symbol
            one-of  < > + - / * = <= >= ( ) { } [ ] != == ; , 

keyword
            one-of  const int char if else while enum debug

---------------------------------------------------------------------------

program
           global-declaration-list

global-declaration
           declaration
           function-definition

global-declaration-list
           global-declaration global-declaration-list
           global-declaration

function-definition
           identifier( function-argument-declaration-list ) function-compound-statement
           identifier( ) function-compound-statement

function-compound-statement
           { local-declaration-list statement-list } 
           { statement-list } 

declaration
           type-specifier identifier ;
           type-specifier identifier [ integer-constant ] ;
           type-specifier identifier [ identifier ] ;
           enum { enumerator-list } ;

local-declaration-list
           declaration local-declaration-list
           declaration

enumerator
           identifier = integer-constant
           identifier

enumerator-list
           enumerator , enumerator-list
           enumerator
           
function-argument-declaration-list
           function-argument-declaration , function-argument-declaration-list
           function-argument-declaration

function-argument-declaration
           type-specifier identifier
           type-specifier identifier []

type-specifier
           int
           char

statement
          if-statement
          while-statement
          compound-statement
          assignment-statement ;
          expression-statement ;
          return-statement ;

statement-list
           statement statement-list
           statement
           
if-statement
           if ( expression ) statement else statement
           if ( expression ) statement

while-statement
           while ( expression ) statement

assignment-statement
           identifier = expression
           identifier [ expression ] = expression
           
expression-statement
          expression

return-statement
           return expression
           return

compound-statement
           { statement-list } 

expression
           logical-or-expression

logical-or-expression
           logical-and-expression logical-or-switch-sequence
           logical-and-expression

logical-or-switch-sequence
           || logical-and-expression logical-or-switch-sequence
           || logical-and-expression

logical-and-expression
           equality-expression logical-and-switch-sequence
           equality-expression

logical-and-switch-sequence
           && equality-expression logical-and-switch-sequence
           && equality-expression

equality-expression
           relational-expression equality-expression-sequence
           relational-expression

equality-expression-sequence
           equality-operator-operator relational-expression equality-expression-sequence
           equality-operator-operator relational-expression

equality-operator
           one-of == !=

relational-expression
           additive-expression relational-expression-sequence
           additive-expression

relational-expression-sequence
           relational-operator additive-expression relational-expression-sequence
           relational-operator additive-expression

relational-operator
          one-of < <= > >=

additive-expression
           multiplicative-expression additive-expression-sequence
           multiplicative-expression

additive-expression-sequence
          additive-operator multiplicative-expression additive-expression-sequence
          additive-operator multiplicative-expression

additive-operator
          one-of  + - 
           
multiplicative-expression
           unary-expression multiplicative-expression-sequence
           unary-expression

multiplicative-expression-sequence
           multiplicative-operator unary-expression multiplicative-expression-sequence
           multiplicative-operator unary-expression

multiplicative-operator
          one-of  * / 

unary-expression
           unary-operator unary-expression
           primary-expression
           
unary-operator
           one of  ! + - 

primary-expression
           built-in-function
           identifier ( function-call-argument-list )
           identifier [ expression ]
           identifier
           integer-constant
           character-constant
           ( expression )

function-call-argument-list
           function-call-argument , function-call-argument-list
           function-call-argument

function-call-argument
           string-literal
           pointer-identifier
           expression

built-in-function
          fgetc ( stdin )
          fputc ( expression , stdout)
          fputc ( expression , stderr)
          exit  ( expression )
          debug ( expression )

``` 
 