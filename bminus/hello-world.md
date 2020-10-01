
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

Hello world compiled to linux elf

```
$bin/bminus_linuxelfx86 <hello-world/hello_world.c >test-temp/hello_world
```
```
$readelf -a test-temp/hello_world
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Intel 80386
  Version:                           0x1
  Entry point address:               0x10000094
  Start of program headers:          52 (bytes into file)
  Start of section headers:          0 (bytes into file)
  Flags:                             0x0
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         3
  Size of section headers:           0 (bytes)
  Number of section headers:         0
  Section header string table index: 0

There are no sections in this file.

There are no sections to group in this file.

Program Headers:
  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
  LOAD           0x000000 0x10000000 0x10000000 0x00197 0x00197 R E 0
  LOAD           0x001000 0x20000000 0x20000000 0x00034 0x00034 RW  0
  LOAD           0x000000 0x30000000 0x30000000 0x00000 0x00000 RW  0

There is no dynamic section in this file.

There are no relocations in this file.

The decoding of unwind sections for machine type Intel 80386 is not currently supported.

Dynamic symbol information is not available for displaying symbols.

No version information found in this file.
```
```
$objdump -D -Matt,i386 -b binary -m i386 --start-address 0x94 --stop-address 0x197 test-temp/hello_world

test-temp/hello_world:     file format binary


Disassembly of section .data:

00000094 <.data+0x94>:
      94:	e8 0c 00 00 00       	call   0xa5
      99:	b8 01 00 00 00       	mov    $0x1,%eax
      9e:	bb 00 00 00 00       	mov    $0x0,%ebx
      a3:	cd 80                	int    $0x80
      a5:	c8 20 00 00          	enter  $0x20,$0x0
      a9:	8d 05 00 00 00 20    	lea    0x20000000,%eax
      af:	89 85 fc ff ff ff    	mov    %eax,-0x4(%ebp)
      b5:	8b 85 fc ff ff ff    	mov    -0x4(%ebp),%eax
      bb:	50                   	push   %eax
      bc:	e8 13 00 00 00       	call   0xd4
      c1:	83 c4 04             	add    $0x4,%esp
      c4:	b8 00 00 00 00       	mov    $0x0,%eax
      c9:	89 c3                	mov    %eax,%ebx
      cb:	b8 01 00 00 00       	mov    $0x1,%eax
      d0:	cd 80                	int    $0x80
      d2:	c9                   	leave  
      d3:	c3                   	ret    
      d4:	c8 24 00 00          	enter  $0x24,$0x0
      d8:	b8 00 00 00 00       	mov    $0x0,%eax
      dd:	89 85 fc ff ff ff    	mov    %eax,-0x4(%ebp)
      e3:	8b 85 fc ff ff ff    	mov    -0x4(%ebp),%eax
      e9:	89 c3                	mov    %eax,%ebx
      eb:	8d 04 9d 00 00 00 00 	lea    0x0(,%ebx,4),%eax
      f2:	89 85 f8 ff ff ff    	mov    %eax,-0x8(%ebp)
      f8:	8b 85 08 00 00 00    	mov    0x8(%ebp),%eax
      fe:	03 85 f8 ff ff ff    	add    -0x8(%ebp),%eax
     104:	89 c3                	mov    %eax,%ebx
     106:	8b 03                	mov    (%ebx),%eax
     108:	89 85 f8 ff ff ff    	mov    %eax,-0x8(%ebp)
     10e:	b8 00 00 00 00       	mov    $0x0,%eax
     113:	2b 85 f8 ff ff ff    	sub    -0x8(%ebp),%eax
     119:	0f 85 0a 00 00 00    	jne    0x129
     11f:	b8 00 00 00 00       	mov    $0x0,%eax
     124:	e9 05 00 00 00       	jmp    0x12e
     129:	b8 01 00 00 00       	mov    $0x1,%eax
     12e:	83 f8 00             	cmp    $0x0,%eax
     131:	0f 84 5e 00 00 00    	je     0x195
     137:	8b 85 fc ff ff ff    	mov    -0x4(%ebp),%eax
     13d:	89 c3                	mov    %eax,%ebx
     13f:	8d 04 9d 00 00 00 00 	lea    0x0(,%ebx,4),%eax
     146:	89 85 f8 ff ff ff    	mov    %eax,-0x8(%ebp)
     14c:	8b 85 08 00 00 00    	mov    0x8(%ebp),%eax
     152:	03 85 f8 ff ff ff    	add    -0x8(%ebp),%eax
     158:	89 c3                	mov    %eax,%ebx
     15a:	8b 03                	mov    (%ebx),%eax
     15c:	50                   	push   %eax
     15d:	b8 04 00 00 00       	mov    $0x4,%eax
     162:	bb 01 00 00 00       	mov    $0x1,%ebx
     167:	89 e1                	mov    %esp,%ecx
     169:	ba 01 00 00 00       	mov    $0x1,%edx
     16e:	cd 80                	int    $0x80
     170:	83 c4 04             	add    $0x4,%esp
     173:	8b 85 fc ff ff ff    	mov    -0x4(%ebp),%eax
     179:	89 85 f8 ff ff ff    	mov    %eax,-0x8(%ebp)
     17f:	b8 01 00 00 00       	mov    $0x1,%eax
     184:	03 85 f8 ff ff ff    	add    -0x8(%ebp),%eax
     18a:	89 85 fc ff ff ff    	mov    %eax,-0x4(%ebp)
     190:	e9 4e ff ff ff       	jmp    0xe3
     195:	c9                   	leave  
     196:	c3                   	ret    

```
```
$ hexdump -C -s 0x1000 test-temp/hello_world 
00001000  48 00 00 00 65 00 00 00  6c 00 00 00 6c 00 00 00  |H...e...l...l...|
00001010  6f 00 00 00 20 00 00 00  77 00 00 00 6f 00 00 00  |o... ...w...o...|
00001020  72 00 00 00 6c 00 00 00  64 00 00 00 0a 00 00 00  |r...l...d.......|
00001030  00 00 00 00                                       |....|
00001034
```

GCC compiling Hello world to assembler:

```
$gcc -w -S -include stdio.h hello-world/hello_world.c -o test-temp/hello_world.asm
```
```
        .file   "hello_world.c"
        .text
        .section        .rodata
.LC0:
        .string "Hello world\n"
        .text
        .globl  main
        .type   main, @function
main:
.LFB0:
        .cfi_startproc
        endbr64
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        leaq    .LC0(%rip), %rdi
        movl    $0, %eax
        call    write
        movl    $0, %edi
        call    exit@PLT
        .cfi_endproc
.LFE0:
        .size   main, .-main
        .globl  write
        .type   write, @function
write:
.LFB1:
        .cfi_startproc
        endbr64
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        subq    $32, %rsp
        movq    %rdi, -24(%rbp)
        movl    $0, -4(%rbp)
        jmp     .L3
.L4:
        movq    stdout(%rip), %rdx
        movl    -4(%rbp), %eax
        movslq  %eax, %rcx
        movq    -24(%rbp), %rax
        addq    %rcx, %rax
        movzbl  (%rax), %eax
        movsbl  %al, %eax
        movq    %rdx, %rsi
        movl    %eax, %edi
        call    fputc@PLT
        addl    $1, -4(%rbp)
.L3:
        movl    -4(%rbp), %eax
        movslq  %eax, %rdx
        movq    -24(%rbp), %rax
        addq    %rdx, %rax
        movzbl  (%rax), %eax
        testb   %al, %al
        jne     .L4
        nop
        leave
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE1:
        .size   write, .-write
        .ident  "GCC: (Ubuntu 9.3.0-10ubuntu2) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
        .section        .note.gnu.property,"a"
        .align 8
        .long    1f - 0f
        .long    4f - 1f
        .long    5
0:
        .string  "GNU"
1:
        .align 8
        .long    0xc0000002
        .long    3f - 2f
2:
        .long    0x3
3:
        .align 8
```


