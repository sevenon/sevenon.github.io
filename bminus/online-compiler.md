
B-minus can compile itself to Javascript and run inside a web browser:

- [Online compiler generating Linux x86 assembler](online-compiler/compile_in_browser_to_linuxassemblerx86.html)

- [Online compiler generating ANSI-C](online-compiler/compile_in_browser_to_cvirtualmachine.html)

- [Online compiler generating Javascript](online-compiler/compile_in_browser_to_javascript.html)


Four different I/O adapters are provided to use with the generated Javascript code:

1. adapter for a web browser
2. adapter for the Oracle JDK "jjs"
3. adapter for the Node.js "nodejs"
4. adapter for Microsoft Windows "cscript"

<!-- more -->

The following is the I/O adapter that lets the generated Javascript code run inside a web browser

```js
var stdin_str = "";
var stdout_str = "";
var stderr_str = "";
var stdin_pos = 0;
var exit_code = 0;

function compile() {
    stdout_str = "";
    stderr_str = "";
    stdin_pos = 0;
    exit_code = 0;

    try {
        init_memory();
        main();
    } catch(e) {
    }
}

function readCharCodeFromStdin() {
    if (stdin_pos < stdin_str.length)
        return (stdin_str.charAt(stdin_pos++)).charCodeAt(0);
    else
        return -1;
}
    
function printCharCodeToStdout(c) {
    stdout_str += String.fromCharCode(c);
}
    
function printCharCodeToStderr(c) {
    stderr_str += String.fromCharCode(c);
}
         
function abortProgram(n) {
    exit_code = n;
    throw false;
}
    
function printIntegerNumber (n) {
    stdout_str += Number(n).toFixed().toString() + "\n";
}

```
