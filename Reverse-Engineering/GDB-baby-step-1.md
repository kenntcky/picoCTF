# GDB Baby Step 1

Contents:
- [Challenge Information](#challenge-information)
- [Solution](#solution)
- [References](#references)

# Challenge Information

![image](https://github.com/user-attachments/assets/02c07873-7371-4429-8769-ee2f70bb184e)
Challenge link: https://play.picoctf.org/practice/challenge/395

# Solution

Firstly, we are given a file called debugger0_a.<br/>
From the title of the challenge, we should already know that we are gonna use GDB to solve this challenge.<br/>
Before we do anything, we will make the file executable first, and then start `gdb` to debug the file as instructed.
```zsh
┌──(kali㉿kali)-[~/learn-ctf/Reverse-Engineering/GDB-baby-step/1]
└─$ chmod +x debugger0_a                            
                                                                                              
┌──(kali㉿kali)-[~/learn-ctf/Reverse-Engineering/GDB-baby-step/1]
└─$ gdb            
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
Type "apropos word" to search for commands related to "word".
(gdb)
```
From the instructions, we are told to find what is in the `eax` register.

We will disassemble the main function:
```
(gdb) disas main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
```

We can see that the `mov` instructions were used to move the hexadecimal value `0x86342` into the `EAX` register.
With that, we can assume that the EAX register contains the hexadecimal value `0x86342`.

Then we just need to convert those hexadecimal to a decimal, and put it in the picoCTF{} flag format.
Voila, we just solved the challenge.

# References

https://www.cs.virginia.edu/~evans/cs216/guides/x86.html
