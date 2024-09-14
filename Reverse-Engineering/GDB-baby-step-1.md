# GDB Baby Step 1

Contents:
- [Challenge Information](#challenge-information)
- [Solution](#solution)
- [References](#references)

# Challenge Information

![image](https://github.com/user-attachments/assets/02c07873-7371-4429-8769-ee2f70bb184e)
Challenge link: https://play.picoctf.org/practice/challenge/395

# Solution

Firstly, we are given a file called debugger0_a. 
From the title of the challenge, we should already know that we are gonna use GDB to solve this challenge.
Before we do anything, we will make the file executable first, and then start `gdb` to debug the file as instructed.
We are prompt to find what is in the `eax` register.

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
