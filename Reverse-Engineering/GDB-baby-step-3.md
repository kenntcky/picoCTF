<h1 align="center">GDB baby step 3</h1>

Contents:
- [Challenge Information](#challenge-information)
- [Solution](#solution)

# Challenge Information

![image](https://github.com/user-attachments/assets/d8d0b746-54c6-41d3-b5aa-8b82ab82dc18)

Challenge link: https://play.picoctf.org/practice/challenge/397

# Solution

We are given a file called debugger0_c.<br/>

Let's make the file executable, and run `gdb` first.<br/>

```zsh
┌──(kali㉿kali)-[~/…/Reverse-Engineering/picoCTF/GDB-baby-step/3]
└─$ chmod +x debugger0_c 
                                                                                              
┌──(kali㉿kali)-[~/…/Reverse-Engineering/picoCTF/GDB-baby-step/3]
└─$ gdb debugger0_c 
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
Reading symbols from debugger0_c...
(No debugging symbols found in debugger0_c)
(gdb)
```

Next, we will disassemble the main function to determine where `0x2262c96b` is loaded in the memory.

```zsh
(gdb) disas main
Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   %rbp
   0x000000000040110b <+5>:     mov    %rsp,%rbp
   0x000000000040110e <+8>:     mov    %edi,-0x14(%rbp)
   0x0000000000401111 <+11>:    mov    %rsi,-0x20(%rbp)
   0x0000000000401115 <+15>:    movl   $0x2262c96b,-0x4(%rbp)
   0x000000000040111c <+22>:    mov    -0x4(%rbp),%eax
   0x000000000040111f <+25>:    pop    %rbp
   0x0000000000401120 <+26>:    ret
End of assembler dump.
```

Alright, now we see that `0x2262c96b` is loaded in `-0x4(%rbp)`.<br/>

Let's put a breakpoint right after the value has been moved into memory, and print out the four bytes that is stored in the memory.

```zsh
(gdb) break *main+22
Breakpoint 1 at 0x40111c
(gdb) r
Starting program: /home/kali/learn-ctf/Reverse-Engineering/picoCTF/GDB-baby-step/3/debugger0_c 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x000000000040111c in main ()
(gdb) x/4xb $rbp-0x4
0x7fffffffdd0c: 0x6b    0xc9    0x62    0x22
```

Boom, we got the flag. Lastly, we just need to input those bytes into the picoCTF{} flag format as told in the challenge description, and you're done!
