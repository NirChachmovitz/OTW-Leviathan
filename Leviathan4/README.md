# Leviathan 4 - Walkthrough

inside the home directory there is a directory called `.trash`. 
Inside there is a file called `bin`:

```
leviathan4@leviathan:~$ ls -la
total 24
drwxr-xr-x  3 root root       4096 Aug 26  2019 .
drwxr-xr-x 10 root root       4096 Aug 26  2019 ..
-rw-r--r--  1 root root        220 May 15  2017 .bash_logout
-rw-r--r--  1 root root       3526 May 15  2017 .bashrc
-rw-r--r--  1 root root        675 May 15  2017 .profile
dr-xr-x---  2 root leviathan4 4096 Aug 26  2019 .trash
leviathan4@leviathan:~$ cd .trash
leviathan4@leviathan:~/.trash$ ls
bin
leviathan4@leviathan:~/.trash$ ls -la
total 16
dr-xr-x--- 2 root       leviathan4 4096 Aug 26  2019 .
drwxr-xr-x 3 root       root       4096 Aug 26  2019 ..
-r-sr-x--- 1 leviathan5 leviathan4 7352 Aug 26  2019 bin
```

Let's run it:

```
leviathan4@leviathan:~/.trash$ ./bin
01010100 01101001 01110100 01101000 00110100 01100011 01101111 01101011 01100101 01101001 00001010
```

I think it's an ascii that represents the password.
Let's google how to make it ascii and figure it out:

```
leviathan4@leviathan:~/.trash$ ./bin | perl -lape '$_=pack"(B8)*",@F'
Tith4cokei
```

Is that the password? I think so.. :)
Well it didn't work. probably because of the endline. let's copy it in a file and move it as a password.

```
nir@LAPTOP-8C4IQCNL:/mnt/c/tmp$ sshpass -f password ssh -l leviathan5 -p 2223 leviathan.labs.overthewire.org
```

Worked!