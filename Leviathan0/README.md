# Leviathan 0 - Walkthrough

First, I logged in using ssh:

`ssh -l leviathan0 -p 2223 leviathan.labs.overthewire.org` 
Password: leviathan0

I searched for the files in the current directory, but found nothing.
Than I went to /etc/leviathan_pass/leviathan1 but of course no permissions was granted.

So, I wrote again `ls -al`, and now I found an interesting folder, called `.backup`.
cd inside, and there is an html file.

I printed it, and lot of gibrish was printed.
I used grep for leviathan, and found:

`<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>`

And indeed, the password was `rioGegei8m` for the next level! :)