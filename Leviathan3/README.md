# Leviathan 3 - Walkthrough

inside the home directory there is a setuid elf called `level3`, owned by `leviathan4`.

`leviathan3@leviathan:~$ ls`
`level3`

Let's run it:

`leviathan3@leviathan:~$ ./level3`
`Enter the password> efefw`
`bzzzzzzzzap. WRONG`

Ok.. Try ltrace and understand:

leviathan3@leviathan:~$ ltrace ./level3
__libc_start_main(0x8048618, 1, 0xffffd784, 0x80486d0 <unfinished ...>
strcmp("h0no33", "kakaka")                                                                            = -1
printf("Enter the password> ")                                                                        = 20
fgets(Enter the password> hello
"hello\n", 256, 0xf7fc55a0)                                                                     = 0xffffd590
strcmp("hello\n", "snlprintf\n")                                                                      = -1
puts("bzzzzzzzzap. WRONG"bzzzzzzzzap. WRONG
)                                                                            = 19
+++ exited (status 0) +++

It seems as it compares the string with `snlprintf`.  Let's put it as an input:

leviathan3@leviathan:~$ ./level3
Enter the password> snlprintf
[You've got shell]!
$ ls
level3
$ ./level3
Enter the password> snlprintf
[You've got shell]!
$ cat /etc/leviathan_pass/leviathan4
vuH0coox6m

Woohoo!