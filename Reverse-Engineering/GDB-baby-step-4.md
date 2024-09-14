<h1 align="center">GDB baby step 4</h1>

Contents:
- [Challenge Information](#challenge-information)
- [Solution](#solution)

# Challenge Information

![image](https://github.com/user-attachments/assets/efb7ce7a-e512-4914-b0eb-5d8e2242efa7)

Challenge link: https://play.picoctf.org/practice/challenge/398

# Solution

We are given a file called debugger0_d.

Let's make the file executable and run `gdb`.

```
┌──(kali㉿kali)-[~/…/Reverse-Engineering/picoCTF/GDB-baby-step/4]
└─$ chmod +x debugger0_d
                                                                                              
┌──(kali㉿kali)-[~/…/Reverse-Engineering/picoCTF/GDB-baby-step/4]
└─$ gdb debugger0_d 
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
Reading symbols from debugger0_d...
(No debugging symbols found in debugger0_d)
(gdb)
```

From the challenge description, the `main` function calls a function that multiplies the `eax` register by a constatn.

We need to find the constant that multiplied `eax` in order to solve the challenge. Let's disassemble `main` function first.

```zsh
(gdb) disas main
Dump of assembler code for function main:
   0x000000000040111c <+0>:     endbr64
   0x0000000000401120 <+4>:     push   %rbp
   0x0000000000401121 <+5>:     mov    %rsp,%rbp
   0x0000000000401124 <+8>:     sub    $0x20,%rsp
   0x0000000000401128 <+12>:    mov    %edi,-0x14(%rbp)
   0x000000000040112b <+15>:    mov    %rsi,-0x20(%rbp)
   0x000000000040112f <+19>:    movl   $0x28e,-0x4(%rbp)
   0x0000000000401136 <+26>:    movl   $0x0,-0x8(%rbp)
   0x000000000040113d <+33>:    mov    -0x4(%rbp),%eax
   0x0000000000401140 <+36>:    mov    %eax,%edi
   0x0000000000401142 <+38>:    call   0x401106 <func1>
   0x0000000000401147 <+43>:    mov    %eax,-0x8(%rbp)
   0x000000000040114a <+46>:    mov    -0x4(%rbp),%eax
   0x000000000040114d <+49>:    leave
   0x000000000040114e <+50>:    ret
End of assembler dump.
```

We can see here the `main` function calls a function called `func1`.

```
   0x0000000000401142 <+38>:    call   0x401106 <func1>
```

Let's now disassemble the `func1` function.

```
(gdb) disas func1
Dump of assembler code for function func1:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   %rbp
   0x000000000040110b <+5>:     mov    %rsp,%rbp
   0x000000000040110e <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000401111 <+11>:    mov    -0x4(%rbp),%eax
   0x0000000000401114 <+14>:    imul   $0x3269,%eax,%eax
   0x000000000040111a <+20>:    pop    %rbp
   0x000000000040111b <+21>:    ret
```

We see there the `imul` instruction or the multiply instruction were used.

```
   0x0000000000401114 <+14>:    imul   $0x3269,%eax,%eax
```

It multiplies the `eax` register by the constant of `0x3269`.

We can already guess the flag, right? We just need to convert it to decimal and put it in the picoCTF{} flag format.
