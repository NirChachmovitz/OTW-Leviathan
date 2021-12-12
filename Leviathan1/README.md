# Leviathan 1 - Walkthrough

Inside the program, I typed `ls` and found an elf file called `check`.
I ran it, and it asked for a password, which of course I didn't know.

I ran `gdb check` (had to remember how to use gdb before :)),
and put a breakpoint on main: `b main`.
Unfortunately, I had not symbol table, so i couldn't debug it as I wanted.

I decided to try something different, and ran `ls -l check`, and saw something interesting:
`-r-sr-x--- 1 leviathan2 leviathan1 7452 Aug 26  2019 check`

Meaning, the owner of the file is leviathan2. Interesting. 
I tried running `file check` to see more information about it:
`check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=c735f6f3a3a94adcad8407cc0fda40496fd765dd, not stripped`
setuid ELF? what does it mean?
Well, it means that when this file is run, it should run with the identity of the owner of the file, rather than the identity of the person who runs the program. (taken from quora).
So, now what?
Well, I think it's irrelevant...
I checked for the file at `/home/leviathan2/*`, and it was also owned by the next leviathan (3).
 
Well, so let's return to the debugging way of action.
I typed `disas main` and saw the assembly code:
`Dump of assembler code for function main:
   0x0804853b <+0>:     lea    0x4(%esp),%ecx
   0x0804853f <+4>:     and    $0xfffffff0,%esp
   0x08048542 <+7>:     pushl  -0x4(%ecx)
   0x08048545 <+10>:    push   %ebp
   0x08048546 <+11>:    mov    %esp,%ebp
   0x08048548 <+13>:    push   %ebx
   0x08048549 <+14>:    push   %ecx
   0x0804854a <+15>:    sub    $0x20,%esp
   0x0804854d <+18>:    movl   $0x786573,-0x10(%ebp)
   0x08048554 <+25>:    movl   $0x72636573,-0x17(%ebp)
   0x0804855b <+32>:    movw   $0x7465,-0x13(%ebp)
   0x08048561 <+38>:    movb   $0x0,-0x11(%ebp)
   0x08048565 <+42>:    movl   $0x646f67,-0x1b(%ebp)
   0x0804856c <+49>:    movl   $0x65766f6c,-0x20(%ebp)
   0x08048573 <+56>:    movb   $0x0,-0x1c(%ebp)
   0x08048577 <+60>:    sub    $0xc,%esp
   0x0804857a <+63>:    push   $0x8048690
   0x0804857f <+68>:    call   0x80483c0 <printf@plt>
   0x08048584 <+73>:    add    $0x10,%esp
   0x08048587 <+76>:    call   0x80483d0 <getchar@plt>
   0x0804858c <+81>:    mov    %al,-0xc(%ebp)
   0x0804858f <+84>:    call   0x80483d0 <getchar@plt>
   0x08048594 <+89>:    mov    %al,-0xb(%ebp)
   0x08048597 <+92>:    call   0x80483d0 <getchar@plt>
   0x0804859c <+97>:    mov    %al,-0xa(%ebp)
   0x0804859f <+100>:   movb   $0x0,-0x9(%ebp)
   0x080485a3 <+104>:   sub    $0x8,%esp
   0x080485a6 <+107>:   lea    -0x10(%ebp),%eax
   0x080485a9 <+110>:   push   %eax
   0x080485aa <+111>:   lea    -0xc(%ebp),%eax
   0x080485ad <+114>:   push   %eax
   0x080485ae <+115>:   call   0x80483b0 <strcmp@plt>
   0x080485b3 <+120>:   add    $0x10,%esp
   0x080485b6 <+123>:   test   %eax,%eax
   0x080485b8 <+125>:   jne    0x80485e5 <main+170>
   0x080485ba <+127>:   call   0x80483e0 <geteuid@plt>
   0x080485bf <+132>:   mov    %eax,%ebx
   0x080485c1 <+134>:   call   0x80483e0 <geteuid@plt>
   0x080485c6 <+139>:   sub    $0x8,%esp`
   
`strcmp` is interesting...
let's breakpoint in address 0x080485ae,
see what's on the stack and print it as a string:

`(gdb) x/20x $sp
0xffffd670:     0xffffd69c      0xffffd698      0xffffd6a8      0x0804859c
0xffffd680:     0x00000001      0x00000000      0x65766f6c      0x646f6700
0xffffd690:     0x63657300      0x00746572      0x00786573      0x0072696e
0xffffd6a0:     0xffffd6c0      0x00000000      0x00000000      0xf7e2a286
0xffffd6b0:     0x00000001      0xf7fc5000      0x00000000      0xf7e2a286`

Well the address looks weird.
We take a look again at the disas, and see that library functions are called.
Let's use ltrace to see it better:
`leviathan1@leviathan:~$ ltrace ./check
__libc_start_main(0x804853b, 1, 0xffffd784, 0x8048610 <unfinished ...>
printf("password: ")                                                                                  = 10
getchar(1, 0, 0x65766f6c, 0x646f6700password:
)                                                                 = 10
getchar(1, 0, 0x65766f6c, 0x646f6700
)                                                                 = 10
getchar(1, 0, 0x65766f6c, 0x646f6700
)                                                                 = 10
strcmp("\n\n\n", "sex")                                                                               = -1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)                                                                  = 29
+++ exited (status 0) +++`

Interesting!
the word is sex. 
Let's try it out:
`leviathan1@leviathan:~$ ./check
password: sex
$`
Okay... we got a shell. I believe it is a levithan2 shell, because of the setuid earlier discussion:
`$ cat /etc/leviathan_pass/leviathan2
ougahZi8Ta`

Finished!