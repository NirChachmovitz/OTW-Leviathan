# Leviathan 6 - Walkthrough

leviathan6@leviathan:~$ ls
leviathan6
leviathan6@leviathan:~$ ./leviathan6
usage: ./leviathan6 <4 digit code>
leviathan6@leviathan:~$ ./leviathan6  1234
Wrong


leviathan6@leviathan:~$ ltrace ./leviathan6
__libc_start_main(0x804853b, 1, 0xffffd784, 0x80485e0 <unfinished ...>
printf("usage: %s <4 digit code>\n", "./leviathan6"usage: ./leviathan6 <4 digit code>
)                                                  = 35
exit(-1 <no return ...>
+++ exited (status 255) +++

Let's figure out what are these 4 digits:


leviathan6@leviathan:~$ gdb leviathan6
GNU gdb (Debian 7.12-6) 7.12.0.20161007-git
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from leviathan6...(no debugging symbols found)...done.
(gdb) disas main
Dump of assembler code for function main:
   0x0804853b <+0>:     lea    0x4(%esp),%ecx
   0x0804853f <+4>:     and    $0xfffffff0,%esp
   0x08048542 <+7>:     pushl  -0x4(%ecx)
   0x08048545 <+10>:    push   %ebp
   0x08048546 <+11>:    mov    %esp,%ebp
   0x08048548 <+13>:    push   %ebx
   0x08048549 <+14>:    push   %ecx
   0x0804854a <+15>:    sub    $0x10,%esp
   0x0804854d <+18>:    mov    %ecx,%eax
   0x0804854f <+20>:    movl   $0x1bd3,-0xc(%ebp)
   0x08048556 <+27>:    cmpl   $0x2,(%eax)
   0x08048559 <+30>:    je     0x804857b <main+64>
   0x0804855b <+32>:    mov    0x4(%eax),%eax
   0x0804855e <+35>:    mov    (%eax),%eax
   0x08048560 <+37>:    sub    $0x8,%esp
   0x08048563 <+40>:    push   %eax
   0x08048564 <+41>:    push   $0x8048660
   0x08048569 <+46>:    call   0x80483b0 <printf@plt>
   0x0804856e <+51>:    add    $0x10,%esp
   0x08048571 <+54>:    sub    $0xc,%esp
   0x08048574 <+57>:    push   $0xffffffff
   0x08048576 <+59>:    call   0x80483f0 <exit@plt>
   0x0804857b <+64>:    mov    0x4(%eax),%eax
   0x0804857e <+67>:    add    $0x4,%eax
   0x08048581 <+70>:    mov    (%eax),%eax
   0x08048583 <+72>:    sub    $0xc,%esp
   0x08048586 <+75>:    push   %eax
   0x08048587 <+76>:    call   0x8048420 <atoi@plt>
   0x0804858c <+81>:    add    $0x10,%esp
   0x0804858f <+84>:    cmp    -0xc(%ebp),%eax
   0x08048592 <+87>:    jne    0x80485bf <main+132>
   0x08048594 <+89>:    call   0x80483c0 <geteuid@plt>
   0x08048599 <+94>:    mov    %eax,%ebx
   0x0804859b <+96>:    call   0x80483c0 <geteuid@plt>
   0x080485a0 <+101>:   sub    $0x8,%esp
   0x080485a3 <+104>:   push   %ebx
   0x080485a4 <+105>:   push   %eax
   0x080485a5 <+106>:   call   0x8048400 <setreuid@plt>
   0x080485aa <+111>:   add    $0x10,%esp
---Type <return> to continue, or q <return> to quit---
   0x080485ad <+114>:   sub    $0xc,%esp
   0x080485b0 <+117>:   push   $0x804867a
   0x080485b5 <+122>:   call   0x80483e0 <system@plt>
   0x080485ba <+127>:   add    $0x10,%esp
   0x080485bd <+130>:   jmp    0x80485cf <main+148>
   0x080485bf <+132>:   sub    $0xc,%esp
   0x080485c2 <+135>:   push   $0x8048682
   0x080485c7 <+140>:   call   0x80483d0 <puts@plt>
   0x080485cc <+145>:   add    $0x10,%esp
   0x080485cf <+148>:   mov    $0x0,%eax
   0x080485d4 <+153>:   lea    -0x8(%ebp),%esp
   0x080485d7 <+156>:   pop    %ecx
   0x080485d8 <+157>:   pop    %ebx
   0x080485d9 <+158>:   pop    %ebp
   0x080485da <+159>:   lea    -0x4(%ecx),%esp
   0x080485dd <+162>:   ret
End of assembler dump.
(gdb) disas main
[1]+  Stopped                 gdb leviathan6

We see at address 0x0804854f that the pin is compared to 0x1bd3, which is 7123 decimal:

leviathan6@leviathan:~$ ./leviathan6 7123
$ cat /etc/leviathan_pass/leviathan7
ahy7MaeBo9
$
