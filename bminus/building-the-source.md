
Compiling and testing B-minus under Linux

(examples are for 32 bit linux, 64 bit linux require 32 bit flags for gcc, ld, and as)

Merge the source code files into one file (bminus does not implement the #include directive)

```sh
cat globals.h stringlib.h errormessages.h preprocessor.h token.h target_linuxassemblerx86.h compiler.h syntax.h bminus.c >bminus_target_x86.c
```

Compile B-minus with gcc

```sh
gcc -include builtinfuncs_ansic.h -o bminus_target_x86 bminus_target_x86.c
```

Compile the hello world example with B-minus

```sh
./bminus_target_x86 <hello_world.c >hello_world.asm
as -o hello_world.o hello_world.asm
ld -o hello_world hello_world.o
```

Run hello world

```sh
./hello_world
```

<!-- more -->

Self compile test

```sh
./bminus_target_x86 <bminus_target_x86.c >bminus_target_x86_self.asm
as -o bminus_target_x86_self.o bminus_target_x86_self.asm
ld -o bminus_target_x86_self bminus_target_x86_self.o
./bminus_target_x86_self <bminus_target_x86.c >bminus_target_x86_self_self.asm
diff bminus_target_x86_self.asm bminus_target_x86_self_self.asm
```

To build the entire project and run all tests

```sh
make clean
make
```

