# How Does My Program Run

## Compiler vs Interpreter

| Type        | Input       | Output                                    | Attribute                                                                             |
| ----------- | ----------- | ----------------------------------------- | ------------------------------------------------------------------------------------- |
| Compiler    | Source Code | Low level Code in B/ByteCode/Machine Code | Take times to compile, fast to execute. Hard to debug, since takes time to see result |
| Interpreter | Source Code | The result of executing the code          | Execute immediatelly, slower than compiled file. Easy to debug                        |

**Compiler**  
Before execution, it takes time to convert the entire source code to low level code(machine code,bytecode, another language). Once compiled, you can even remove original source code, since you run the program by execute the compiled code.

**Interpreter**  
Read source code part by part(maybe line or maybe a block), convert them into lower-level format and execute them on the fly. 

Many language in mordern days takes compiler and interpreter at the same time. Like: Java, python.

## What is JIT?

JIT compilation is a combination of the two traditional approaches of translation – ahead-of-time compilation (AOT), and interpretation. The system implementing the JIT will continuously analyses the running code and identifies the part of the code where the speedup gained from compliation or rearrangement outweight the overhead. For example, optimize the repeatedly used functions or 1000 iteration loops.

## What is Runtime-Enviroment/Virtual Machine?

### RunTime

The time when code is being executed. It is often refered to the runtime enviroment which is the context/enviroment to execute the code.

### VM

- `System VM` provide a substitue of a real machine. It provides funtionality needed to execute entire OS.
- `Process VM` provide a platform-independant programming environment that abstract away the detail of underlying hardware/OS and allows program to execute in same way on any platform. Same language syntax works on differetn platform, as programmer, we don't need to worry about how our code interactive with different OS and platform. Process VM are usually implemented using interpreter, the performance can be improved by the use of JIT compilation.
  
## How Python runs?

Python run-time first compile source code into bytecode(*.pyc), then interpret bytecode and execute in VM.  
There is different implementation of python, interpreter will execute code in different place. cpython -> vm, jyphon -> jvm, pypy use jit and vm.  

## How Java runs?

- Compile java code to bytecode.
- Using JIT to interpret bytecode in JVM.

## How JS runs?

## How Typescript runs?

## How GO runs?

