# Leviathan 2 - Walkthrough

inside the home directory there is a setuid elf called `printfile`, owned by leviathan3.
It requires an argument, so I gave it `/etc/leviathan_pass/leviathan3` and:
`leviathan2@leviathan:~$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...`

Welll...
Running ltrace proves us that it is actually running a `/bin/cat`.