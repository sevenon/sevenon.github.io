
![](images/self-240px.jpg)

# *sevenon's project archive*
https://github.com/sevenon

<br>

# memorabilia

![](images/apple2e-120px.jpg)

Apple IIe - I wrote my first program on an Apple IIe. It was year ten at a Melbourne high school. I was immediately hooked.

<br>

![](images/c128-120px.jpg) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![](images/c64progref-80px.jpg) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![](images/c64mikrogep-80px.jpg)

Shortly after my parents bought me a Commodore 128. I had an amber monochrome monitor, disk drive and a dot matrix printer too.

<br>

My first IT purchase was a book titled: Commodore 64 Programmer's Reference Guide. It's one of the few memorabilia I kept from my teens. A family friend gave me another book titled: A Commodore 64 mikrogep kezelese es programozasa. It was from Hungary.

<br>

![](images/frogger-animation-240px.gif)

I played Frogger a lot, unfortunately it was terribly slow to load from a casette tape. I figured out a way to save the entire game to a floppy disk. I was staring at assembly code for days.

<br>

# toyrouter - a linux router stripped to bare essentials 
https://github.com/sevenon/toyrouter

<br>

*September 2020*

Bare bones Linux WAN to LAN router.

Only uses busybox and iptables in user space. Built as a programming exercise, but functional. Probably the simplest router you will find.

Designed to install over Ubuntu by creating a new entry in the Ubuntu boot menu. Uses overlayfs without modifying the Ubuntu user space. 

Retains the easy installation and hardware compability of Ubuntu with the simplicity of a busybox based userspace.

I got the inspiration from [The Ars guide to building a Linux router from scratch](https://arstechnica.com/gadgets/2016/04/the-ars-guide-to-building-a-linux-router-from-scratch/).
I decided that I wanted something that is even more from "scratch".

<br>

# bminus - a c subset compiler
https://github.com/sevenon/bminus

<br>

*November 2014*

Generates assembler or javascript. Capable of compiling itself from within a web browser.

Straight forward design. BNF rules map to one function in general. Code is generated straight from a recursive parser.

Retargetable - the code generator module can be replaced independently to support multiple output targets.


- [Hello world](bminus/hello-world)
- [Language syntax](bminus/language-syntax)
- [Online compiler](bminus/online-compiler)
- [Source code](bminus/source-code)
- [Building the source](bminus/building-the-source)

<br>

*September 2020*

Compiles under Ubuntu 20.04. Replaced Oracle jjs with NodeJs.



<br>

-----
sevenon.au@gmail.com

