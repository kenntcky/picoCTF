<h1 align="center">GDB baby step 2</h1>

Contents:
- [Challenge Information](#challenge-information)
- [Solution](#solution)

# Challenge Information

![image](https://github.com/user-attachments/assets/ca6d9943-82ac-4b8c-9286-203f9925cb42)

Challenge link: https://play.picoctf.org/practice/challenge/396

# Solution

Firstly, we are given a file called debugger0_b.<br/>

Just like GDB baby step 1, the title is telling us to use GDB to solve this challenge.<br/>

Let's make the file executable, and then start `gdb`.

```
┌──(kali㉿kali)-[~/…/Reverse-Engineering/picoCTF/GDB-baby-step/2]
└─$ chmod +x debugger0_b
                                                                                              
┌──(kali㉿kali)-[~/…/Reverse-Engineering/picoCTF/GDB-baby-step/2]
└─$ gdb debugger0_b 
GNU gdb (Debian 13.2-1+b2) 13.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_b...
(No debugging symbols found in debugger0_b)
(gdb)
```

We need to find out what is in the `eax` register at the end of the `main` function.<br/>

Let's disassemble the `main` function first.

```
(gdb) disas main
Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   %rbp
   0x000000000040110b <+5>:     mov    %rsp,%rbp
   0x000000000040110e <+8>:     mov    %edi,-0x14(%rbp)
   0x0000000000401111 <+11>:    mov    %rsi,-0x20(%rbp)
   0x0000000000401115 <+15>:    movl   $0x1e0da,-0x4(%rbp)
   0x000000000040111c <+22>:    movl   $0x25f,-0xc(%rbp)
   0x0000000000401123 <+29>:    movl   $0x0,-0x8(%rbp)
   0x000000000040112a <+36>:    jmp    0x401136 <main+48>
   0x000000000040112c <+38>:    mov    -0x8(%rbp),%eax
   0x000000000040112f <+41>:    add    %eax,-0x4(%rbp)
   0x0000000000401132 <+44>:    addl   $0x1,-0x8(%rbp)
   0x0000000000401136 <+48>:    mov    -0x8(%rbp),%eax
   0x0000000000401139 <+51>:    cmp    -0xc(%rbp),%eax
   0x000000000040113c <+54>:    jl     0x40112c <main+38>
   0x000000000040113e <+56>:    mov    -0x4(%rbp),%eax
   0x0000000000401141 <+59>:    pop    %rbp
   0x0000000000401142 <+60>:    ret
End of assembler dump.
```

We can see the `eax` register at then end of the `main` function.<br/>

Alright, let's put a breakpoint at the end of the `main` function.

```
(gdb) break *main+60
Breakpoint 1 at 0x401142
```

Next, we will run the program and inspect the value of the `eax` register.

```
(gdb) r
Starting program: /home/kali/learn-ctf/Reverse-Engineering/picoCTF/GDB-baby-step/2/debugger0_b 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x0000000000401142 in main ()
(gdb) info registers eax
eax            0x4af4b             307019
```

Voila, we got the value of the `eax` register both in hexadecimal and decimal value.<br/>

Lastly, we just need to put the decimal value in the picoCTF{} flag format.
