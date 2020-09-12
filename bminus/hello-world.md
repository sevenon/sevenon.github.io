
Features

- Capable of compiling itself

- Can be compiled with a standard C compiler

- Self contained, does not use the C library

- Targets supported:

	- Linux X86 Assembler

	- CPU emulator implemented in ANSI C

	- CPU emulator implemented in Javascript


Designe objectives:

1. **Implement just enough features from the C language to be able to self compile**

2. **Simplicity and readability first, speed and efficiency last**

<br>
<br>


Example for "Hello world" compiled to different targets

```c
main() {

    write("Hello world\n");
    exit(0);
}

write(char str[]) {
	int i;

	i = 0;
	while (str[i] != 0) {
		fputc(str[i], stdout);
		i = i + 1;
	}
}
```
<!--more-->

"Hello world" compiled to Linux X86 assembler

```
.text
.globl _start
_start:
                        call              main
                        movl              $1, %eax
                        movl              $0, %ebx
                        int               $0x80

                                                            # 
                                                            # main() {
main:                   enter             $32, $0
                                                            # 
                                                            #     write("Hello world\n");
                        lea               4+string, %eax
                        movl              %eax, -4(%ebp)
                        movl              -4(%ebp), %eax
                        pushl             %eax
                        call              write
                        addl              $4, %esp
                                                            #     exit(0);
                        movl              $0, %eax
                        movl              %eax, %ebx
                        movl              $1, %eax
                        int               $0x80
                                                            # }
                                                            # 
                                                            # write(char str[]) {
main_end:
                        leave
                        ret
write:                  enter             $36, $0
                                                            # 	int i;
                                                            # 
                                                            # 	i = 0;
                        movl              $0, %eax
                        movl              %eax, -4(%ebp)
                                                            # 	while (str[i] != 0) {
while_0_test:           movl              -4(%ebp), %eax
                        movl              %eax, %ebx
                        lea               (,%ebx,4), %eax
                        movl              %eax, -8(%ebp)
                        movl              8(%ebp), %eax
                        addl              -8(%ebp), %eax
                        movl              %eax, %ebx
                        movl              (%ebx), %eax
                        movl              %eax, -8(%ebp)
                        movl              $0, %eax
                        subl              -8(%ebp), %eax
                        jne               compare_1_true
                        movl              $0, %eax
                        jmp               compare_1_false
compare_1_true:         movl              $1, %eax
compare_1_false:        cmpl              $0, %eax
                        jz                while_0_end
                                                            # 		fputc(str[i], stdout);
                        movl              -4(%ebp), %eax
                        movl              %eax, %ebx
                        lea               (,%ebx,4), %eax
                        movl              %eax, -8(%ebp)
                        movl              8(%ebp), %eax
                        addl              -8(%ebp), %eax
                        movl              %eax, %ebx
                        movl              (%ebx), %eax
                        pushl             %eax
                        movl              $4, %eax
                        movl              $1, %ebx
                        movl              %esp, %ecx
                        movl              $1, %edx
                        int               $0x80
                        addl              $4, %esp
                                                            # 		i = i + 1;
                        movl              -4(%ebp), %eax
                        movl              %eax, -8(%ebp)
                        movl              $1, %eax
                        addl              -8(%ebp), %eax
                        movl              %eax, -4(%ebp)
                                                            # 	}
                                                            # }
                        jmp               while_0_test
while_0_end:
write_end:
                        leave
                        ret
.bss
global:
.lcomm global_storage_space, 0

.data
string:
.int 0
.int 72 # 'H'
.int 101 # 'e'
.int 108 # 'l'
.int 108 # 'l'
.int 111 # 'o'
.int 32 # ' '
.int 119 # 'w'
.int 111 # 'o'
.int 114 # 'r'
.int 108 # 'l'
.int 100 # 'd'
.int 10
.int 0
.end

```

"Hello world" compiled to a CPU emulator implemented in ANSI-C

```c
#include <stdio.h>
#include <stdlib.h>

int A;
int X;
extern int BP;
extern int RAM[];

loadi       (int n) { A = n; }

load        (int n) { A = RAM[n]; }
store       (int n) { RAM[n] = A; }

add         (int n) { A += RAM[n]; }
subtract    (int n) { A -= RAM[n]; }
multiply    (int n) { A *= RAM[n]; }
divide      (int n) { A /= RAM[n]; }

loadx       ()      { X = A; }
movebp      (int n) { BP += n; }

less        ()      { return A > 0; }
greater     ()      { return A < 0; }
equals      ()      { return A == 0; }
notequals   ()      { return A != 0; }
always      ()      { return 1; }

extern int global;

                                                            // 
                                                            // main() {
main
(){
                                                            // 
                                                            //     write("Hello world\n");
                        loadi             (0);
                        store             (0+BP);
                        load              (0+BP);
                        store             (9+BP);
                        movebp            (9);
                        write();          
                        movebp            (-9);
                                                            //     exit(0);
                        loadi             (0);
                        exit              (A);
                                                            // }
                                                            // 
                                                            // write(char str[]) {
main_end:
return;	}
write
(){
                                                            // 	int i;
                                                            // 
                                                            // 	i = 0;
                        loadi             (0);
                        store             (1+BP);
                                                            // 	while (str[i] != 0) {
while_0_test:           load              (1+BP);
                        loadx             ();
                        loadi             (X);
                        store             (2+BP);
                        load              (0+BP);
                        add               (2+BP);
                        loadx             ();
                        load              (X);
                        store             (2+BP);
                        loadi             (0);
                        subtract          (2+BP);
                        if(notequals())   goto compare_1_true;
                        loadi             (0);
                        if(always())      goto compare_1_false;
compare_1_true:         loadi             (1);
compare_1_false:        if(equals())      goto while_0_end;
                                                            // 		fputc(str[i], stdout);
                        load              (1+BP);
                        loadx             ();
                        loadi             (X);
                        store             (2+BP);
                        load              (0+BP);
                        add               (2+BP);
                        loadx             ();
                        load              (X);
                        fputc             (A, stdout);
                                                            // 		i = i + 1;
                        load              (1+BP);
                        store             (2+BP);
                        loadi             (1);
                        add               (2+BP);
                        store             (1+BP);
                                                            // 	}
                                                            // }
                        if(always())      goto while_0_test;
while_0_end:
write_end:
return;	}


int global = 1048589;

int BP = 13;

int RAM[1048589] = {
'H',
'e',
'l',
'l',
'o',
' ',
'w',
'o',
'r',
'l',
'd',
10,
0
};

```

"Hello world" compiled to a CPU emulator implemented in Javascript

```js
var A;
var X;
var BP;
var RAM = [];

function loadi       (n) { A = n; }

function load        (n) { A = RAM[n]; }
function store       (n) { RAM[n] = A; }

function add         (n) { A += RAM[n]; }
function subtract    (n) { A -= RAM[n]; }
function multiply    (n) { A *= RAM[n]; }
function divide      (n) { A = (A / RAM[n]) | 0; }

function loadx       ()  { X = A; }
function movebp      (n) { BP += n; }

function less        ()  { return A > 0; }
function greater     ()  { return A < 0; }
function equals      ()  { return A == 0; }
function notequals   ()  { return A != 0; }
function always      ()  { return true; }

var global;

                                                            // 
                                                            // main() {
function main
(){
                                             main_end: {
                                                            // 
                                                            //     write("Hello world\n");
                        loadi             (0);
                        store             (0+BP);
                        load              (0+BP);
                        store             (9+BP);
                        movebp            (9);
                        write();          
                        movebp            (-9);
                                                            //     exit(0);
                        loadi             (0);
                        abortProgram      (A);
                                                            // }
                                                            // 
                                                            // write(char str[]) {
} // main_end:
return;	}
function write
(){
                                             write_end: {
                                                            // 	int i;
                                                            // 
                                                            // 	i = 0;
                        loadi             (0);
                        store             (1+BP);
                                                            // 	while (str[i] != 0) {
                                             while_0_end: {
while_0_test: while(true) {
                        load              (1+BP);
                        loadx             ();
                        loadi             (X);
                        store             (2+BP);
                        load              (0+BP);
                        add               (2+BP);
                        loadx             ();
                        load              (X);
                        store             (2+BP);
                        loadi             (0);
                        subtract          (2+BP);
                                             compare_1_false: {
                                             compare_1_true: {
                        if(notequals())   break compare_1_true;
                        loadi             (0);
                        if(always())      break compare_1_false;
} // compare_1_true:
                        loadi             (1);
} // compare_1_false:
                        if(equals())      break while_0_end;
                                                            // 		fputc(str[i], stdout);
                        load              (1+BP);
                        loadx             ();
                        loadi             (X);
                        store             (2+BP);
                        load              (0+BP);
                        add               (2+BP);
                        loadx             ();
                        load              (X);
                        printCharCodeToStdout (A);
                                                            // 		i = i + 1;
                        load              (1+BP);
                        store             (2+BP);
                        loadi             (1);
                        add               (2+BP);
                        store             (1+BP);
                                                            // 	}
                                                            // }
                        if(always())      continue while_0_test;
                                             break; } // while_0_test:
} // while_0_end:
} // write_end:
return;	}


function init_memory() {
global = 1048589;

BP = 13;

RAM = [
"H".charCodeAt(0),
"e".charCodeAt(0),
"l".charCodeAt(0),
"l".charCodeAt(0),
"o".charCodeAt(0),
" ".charCodeAt(0),
"w".charCodeAt(0),
"o".charCodeAt(0),
"r".charCodeAt(0),
"l".charCodeAt(0),
"d".charCodeAt(0),
10,
0
];

for (i = 13; i < 1048589; i ++) {
RAM[i] = 0;
}
}

```


just for comparison, this is how Visual C++ compiles "Hello world"

```
; Listing generated by Microsoft (R) Optimizing Compiler Version 16.00.30319.01 

	TITLE	C:\Work\bminus\hello-world\hello_world.c
	.686P
	.XMM
	include listing.inc
	.model	flat

INCLUDELIB MSVCRT
INCLUDELIB OLDNAMES

PUBLIC	_write
EXTRN	__imp__fputc:PROC
EXTRN	__imp____iob_func:PROC
; Function compile flags: /Ogsp
; File c:\work\bminus\hello-world\hello_world.c
;	COMDAT _write
_TEXT	SEGMENT
_str$ = 8						; size = 4
_write	PROC						; COMDAT

; 8    : write(char str[]) {

	push	ebp
	mov	ebp, esp
	push	esi

; 9    : 	int i;
; 10   : 
; 11   : 	i = 0;
; 12   : 	while (str[i] != 0) {

	mov	esi, DWORD PTR _str$[ebp]
	jmp	SHORT $LN7@write
$LL2@write:

; 13   : 		fputc(str[i], stdout);

	call	DWORD PTR __imp____iob_func
	add	eax, 32					; 00000020H
	push	eax
	movsx	eax, BYTE PTR [esi]
	push	eax
	call	DWORD PTR __imp__fputc
	pop	ecx
	pop	ecx

; 14   : 		i = i + 1;

	inc	esi
$LN7@write:

; 9    : 	int i;
; 10   : 
; 11   : 	i = 0;
; 12   : 	while (str[i] != 0) {

	cmp	BYTE PTR [esi], 0
	jne	SHORT $LL2@write
	pop	esi

; 15   : 	}
; 16   : }

	pop	ebp
	ret	0
_write	ENDP
_TEXT	ENDS
PUBLIC	??_C@_0N@NLPDAPMJ@Hello?5world?6?$AA@		; `string'
PUBLIC	_main
EXTRN	__imp__exit:PROC
;	COMDAT ??_C@_0N@NLPDAPMJ@Hello?5world?6?$AA@
CONST	SEGMENT
??_C@_0N@NLPDAPMJ@Hello?5world?6?$AA@ DB 'Hello world', 0aH, 00H ; `string'
; Function compile flags: /Ogsp
CONST	ENDS
;	COMDAT _main
_TEXT	SEGMENT
_main	PROC						; COMDAT

; 3    : 
; 4    :     write("Hello world\n");

	push	OFFSET ??_C@_0N@NLPDAPMJ@Hello?5world?6?$AA@
	call	_write
	pop	ecx

; 5    :     exit(0);

	push	0
	call	DWORD PTR __imp__exit
$LN4@main:
$LN3@main:
	int	3
_main	ENDP
_TEXT	ENDS
END

```

