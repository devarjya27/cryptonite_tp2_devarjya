# GDB baby step 1
**Flag:** picoCTF{549698}

## My Solve
As the challenge's names is `GDB baby step` it is clear that we need to use the `gdb` command. 
* Firstly I made the file a executable using `chmod`
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ chmod +x debugger0_a
```
* After that we pass in the name of the program as an argument for `gdb` which will help us debug the program
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ gdb debugger0_a
```
* As per the challenge our required hexadecimal number is in the main function. After looking at the manpage for `gdb` I found out that `break` will be useful. Thus setting a breakpoint at the `main` function will help us look for that required hexadecimal number.
Image
```
gef➤  break main
Breakpoint 1 at 0x1131
```
* After this we simply run the program
```
gef➤  run
```
```
   0x555555555129 <main+0000>      endbr64 
   0x55555555512d <main+0004>      push   rbp
   0x55555555512e <main+0005>      mov    rbp, rsp
 → 0x555555555131 <main+0008>      mov    DWORD PTR [rbp-0x4], edi
   0x555555555134 <main+000b>      mov    QWORD PTR [rbp-0x10], rsi
   0x555555555138 <main+000f>      mov    eax, 0x86342
   0x55555555513d <main+0014>      pop    rbp
   0x55555555513e <main+0015>      ret    
   0x55555555513f                  nop    
```
* Here we can see that `0x86342` is in the `eax` register thus that is our required hexadecimal number
* Now we simply convert that to its decimal counterpart and that will be the content of our flag.
Image

## Incorrect Tangents I Went On
After running the program, i overlooked the `eax` and started finding the decimal equivalents of the memory addresses and kept trying to find the flag with those numbers like a dumbass. Eventually I caught on and found the required flag.

## What I Learned
I already had an idea of `gdb` as I had used it when I participated in `SpookyCTF` and inevitably had to learn it. This challenge gave me clarity on the command.
