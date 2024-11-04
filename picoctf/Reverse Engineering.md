# GDB baby step 1
**Flag:** picoCTF{549698}

## My Solve
As the challenge's names is `GDB baby step 1` it is clear that we need to use the `gdb` command. 
* Firstly I made the file a executable using `chmod`
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ chmod +x debugger0_a
```
* After that we pass in the name of the program as an argument for `gdb` which will help us debug the program
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ gdb debugger0_a
```
* As per the challenge our required hexadecimal number is in the main function. After looking at the manpage for `gdb` I found out that `break` will be useful. Thus setting a breakpoint at the `main` function will help us look for that required hexadecimal number.

![image](https://github.com/user-attachments/assets/20d058f4-3382-429c-8614-a20f955d28a8)

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

![image](https://github.com/user-attachments/assets/05ab0a6b-2cdd-4f73-b4b7-8833b6eb3157)


## Incorrect Tangents I Went On
After running the program, i overlooked the `eax` and started finding the decimal equivalents of the memory addresses and kept trying to find the flag with those numbers like a dumbass. Eventually I caught on and found the required flag.

## What I Learned
I already had an idea of `gdb` as I inevitably had to learn it when I participated in `SpookyCTF`. This challenge gave me clarity on the command.

## References
Manpage for `gdb` <https://manpages.ubuntu.com/manpages/mantic/man1/gdb.1.html>




# ARMssembly 1
**Flag:** picoCTF{0000001b}

## My Solve
In this challenge we are given a piece of code written in assembly. The code takes some input performs certain calculations and prints "You Win" or "You Lose" depending on our input. Our goal is to find the number that manages to print "You Win"
* As I had no idea about assembly, I took help from ChatGPT and <https://www.youtube.com/watch?v=1d-6Hv1c39c> and they explained assembly to me which helped me find the required flag.
* Firstly, in `func` function, we can see where the code stores our fixed integers: 81,0 and 3. We can see that `81` is stored in `[sp, 16]`, `0` is stored in `[sp, 20]` and `3` is stored in `[sp, 24]`.

   ![image](https://github.com/user-attachments/assets/f8d5798a-ac95-46dc-85ec-e3836c22da9b)

* Now it performs certain calculations on these 3 numbers along with the input we provide in `main`. At first it takes the number that is stored in `[sp, 16]` i.e. `81` and stores it in `w1` and stores the number from `[sp, 20]` i.e. `0` and stores it in `w0` and performs `lsl` i.e. a left shift of `w0` i.e. `0` bits and stores the answer in `[sp, 28]`. The answer in this case is `81`.

   ![image](https://github.com/user-attachments/assets/afa81309-a346-4c50-a208-12fa840b34fa)
* After this it loads the number which is stored in `[sp, 28]` i.e. `81` and stores it in `w1` and loads the number stored in `[sp, 24]` i.e. `3` and stores it in `w0`. Then it performs division, it divides `w1` by `w0` and stores the result in `[sp, 28]`. the answer in this case is `27`.

   ![image](https://github.com/user-attachments/assets/9990f861-3730-47ca-8a6d-3f9ec9b44af1)

* Finally it performs subtraction, it loads the the number stored in `[sp, 28]` i.e. `27` and subtracts it by the input argument which is stored in `[sp, 12]`. The answer is again stored in `[sp, 28]` and returned.

   ![image](https://github.com/user-attachments/assets/33e0dec5-6f1b-4eaa-af4c-df404b006618)

* Now we compare the returned value with 0. If it is 0 then the program prints "You Win" otherwise prints "You Lose"

   ![image](https://github.com/user-attachments/assets/ac6b5ac8-f3b5-43e6-97bd-4e0821f17dca)

Hence our input should be `27`. Only if our input is `27` will the program will print "You Win"

![image](https://github.com/user-attachments/assets/ba10cbad-3347-47b2-b9c3-d79c5353b62c)

Now arranging `1b` in the given format, we get `0000001b` as the content of the flag should be a 8 character hexadecimal value 0 padded to 32 bits.

## Incorrect Tangents I Went On
As the code was explained and simplified by ChatGPT I got the flag on my first few tries.

## What I Learned
Learned about Assembly although not much. Atleast now i can explain how values are stored and how simple calculations are performed in Assembly

## References
<https://www.youtube.com/watch?v=1d-6Hv1c39c>



