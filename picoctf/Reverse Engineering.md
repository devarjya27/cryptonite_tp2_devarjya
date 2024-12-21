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


# vault-door-3
**Flag:** picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}

## My Solve
Lookin at the code it is clear that the code expects a input from the user in the format of `picoCTF{` it then takes the substring of the input i.e. the password and checks it in the `checkPassword` function. The function then performs some transformations on it and checks it with `jU5t_a_sna_3lpm18gb41_u_4_mfr340`, if they are the same it returns true and prints "Acces Granted" signifying that we got the flag  right otherwise prints "Access Denied".
* Firstly our password i.e. the substring after `picoCTF{` should be 32 characters long.

  ![image](https://github.com/user-attachments/assets/b3a01ae9-2730-4694-a473-8645f74b6286)
* It then creates a `buffer` string of 32 characters and inserts the first 8 characters of the password into `buffer`.

   ![image](https://github.com/user-attachments/assets/a542319b-87dd-4892-89fd-04a348c3073d)

* It then reverses the characters of password from index 8 to index 15 and puts it in `buffer`.

  ![image](https://github.com/user-attachments/assets/ce96af5a-a870-4bee-b06d-2a8a5f12f342)

* After this the code reverses the even indexed characters from index 16 to index 30 and stores it in the even indexes of `buffer`.

  ![image](https://github.com/user-attachments/assets/1e7ec283-1865-4073-9b1a-58b60bb50223)

* After this the code stores the odd indexed characters from index 17 to index 31 in the odd indexes of `buffer`

  ![image](https://github.com/user-attachments/assets/db76d577-142f-4775-97c2-f21082d8c31e)

* After this it checks the transformed string with `jU5t_a_sna_3lpm18gb41_u_4_mfr340` and returns `true` if they are same or `false` if they are different.

  ![image](https://github.com/user-attachments/assets/55f76700-52c3-4df8-a1fe-32fef075dc09)

I manually found out the password by starting from the last string and following what each loop does, thus giving me the flag.

   ![image](https://github.com/user-attachments/assets/9068bab4-af55-4a4e-8ada-25eb02953d1d)

## Incorrect Tangents I Went On
After doing the entire thing manually I realized i simply could have coded a program that performs the transformations for me taking `jU5t_a_sna_3lpm18gb41_u_4_mfr340` as the input and giving me the password. But regardless the challenge was fun although it took some time as i had to perform the transformations manually

## What I Learned
As I have decent experience with coding in java, understanding what the code did was fairly easy.

## References
<https://www.youtube.com/watch?v=84uM8TIoqcI&list=PLMB3ddm5Yvh3gf_iev78YP5EPzkA3nPdL&index=14>

<https://www.youtube.com/watch?v=gh2RXE9BIN8>

# GDB baby step 2
**Flag:** picoCTF(307019}

## My Solve
Firstly made the program executable using `chmod`.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ chmod +x debugger0_b
```
Then passed the program to `gdb` after that disassembled `main` as it is said that we need to print the value of `eax` register at the end of `main`.
```
gef➤  disassemble main
Dump of assembler code for function main:
   0x0000000000401106 <+0>:	endbr64 
   0x000000000040110a <+4>:	push   rbp
   0x000000000040110b <+5>:	mov    rbp,rsp
   0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x14],edi
   0x0000000000401111 <+11>:	mov    QWORD PTR [rbp-0x20],rsi
   0x0000000000401115 <+15>:	mov    DWORD PTR [rbp-0x4],0x1e0da
   0x000000000040111c <+22>:	mov    DWORD PTR [rbp-0xc],0x25f
   0x0000000000401123 <+29>:	mov    DWORD PTR [rbp-0x8],0x0
   0x000000000040112a <+36>:	jmp    0x401136 <main+48>
   0x000000000040112c <+38>:	mov    eax,DWORD PTR [rbp-0x8]
   0x000000000040112f <+41>:	add    DWORD PTR [rbp-0x4],eax
   0x0000000000401132 <+44>:	add    DWORD PTR [rbp-0x8],0x1
   0x0000000000401136 <+48>:	mov    eax,DWORD PTR [rbp-0x8]
   0x0000000000401139 <+51>:	cmp    eax,DWORD PTR [rbp-0xc]
   0x000000000040113c <+54>:	jl     0x40112c <main+38>
   0x000000000040113e <+56>:	mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401141 <+59>:	pop    rbp
   0x0000000000401142 <+60>:	ret    
End of assembler dump.
```
Here we can see that `main` ends at `+60` so I set a breakpoint just before `main` ends at `+59`.
```
gef➤  break *(main+59)
```
Now as it was asked to print the value of `eax` at the end of main I just simply did `print $eax`
```
gef➤  print $eax
$1 = 0x4af4b
```
Now converting this hexademical number to decimal, we get our `n` after which we wrap it in `picoCTF{n}` to get our flag.

![image](https://github.com/user-attachments/assets/2c6969de-8153-46ec-a2fc-f62b24a06949)

## Incorrect Tangents I Went On
Initially set breakpoint at main without disasssembling and analyzing main, so was not getting to the end of main.

## What I Learned
Learned how to set breakpoints at specific points of a function and learned how to print registers.

# GDB baby step 4
**Flag:** picoCTF{12905}

## My Solve
Firstly made the file executable:
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ chmod +x debugger0_d
```
Now disassembled `main` to see which function multiplies the constant to `eax` and when:
```
gef➤  disassemble main
Dump of assembler code for function main:
   0x000000000040111c <+0>:	endbr64 
   0x0000000000401120 <+4>:	push   rbp
   0x0000000000401121 <+5>:	mov    rbp,rsp
   0x0000000000401124 <+8>:	sub    rsp,0x20
   0x0000000000401128 <+12>:	mov    DWORD PTR [rbp-0x14],edi
   0x000000000040112b <+15>:	mov    QWORD PTR [rbp-0x20],rsi
   0x000000000040112f <+19>:	mov    DWORD PTR [rbp-0x4],0x28e
   0x0000000000401136 <+26>:	mov    DWORD PTR [rbp-0x8],0x0
   0x000000000040113d <+33>:	mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401140 <+36>:	mov    edi,eax
   0x0000000000401142 <+38>:	call   0x401106 <func1>
   0x0000000000401147 <+43>:	mov    DWORD PTR [rbp-0x8],eax
   0x000000000040114a <+46>:	mov    eax,DWORD PTR [rbp-0x4]
   0x000000000040114d <+49>:	leave  
   0x000000000040114e <+50>:	ret    
End of assembler dump.
```
We can see that `func1` is the only function being called in main so its very likely that `func1` multiplies `eax` with the constant. So now i disassembled `func1` look at whats happening inside that function.
```
gef➤  disassemble func1
Dump of assembler code for function func1:
   0x0000000000401106 <+0>:	endbr64 
   0x000000000040110a <+4>:	push   rbp
   0x000000000040110b <+5>:	mov    rbp,rsp
   0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x4],edi
   0x0000000000401111 <+11>:	mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401114 <+14>:	imul   eax,eax,0x3269
   0x000000000040111a <+20>:	pop    rbp
   0x000000000040111b <+21>:	ret    
End of assembler dump.
```
Here we can see `imul` at `+14` and we see that `eax` gets multiplied with `0x3269`. Thus that is our required constant. Now convert it to decimal and wrap it in our flag format to get the flag.

## Incorrect Tangents I Went On
None

## What I Learned
Became more familiar with `gdb`.

# Picker I

**Flag:** picoCTF{4_d14m0nd_1n_7h3_r0ugh_ce4b5d5b}

## My Solve
Looking at the source code provided to use we can see that the program prompts us for a input and calls the function related to our input. For example if our input was `getRandomNumber` it would call `getRandomNumber()`
```
print('Try entering "getRandomNumber" without the double quotes...')
    user_input = input('==> ')
    eval(user_input + '()')
```
Now looking at the source code a bit more we can see that a function `win()` returns `flag.txt` in hexadecimal format
```
def win():
  # This line will not work locally unless you create your own 'flag.txt' in
  #   the same directory as this script
  flag = open('flag.txt', 'r').read()
  #flag = flag[:-1]
  flag = flag.strip()
  str_flag = ''
  for c in flag:
    str_flag += str(hex(ord(c))) + ' '
  print(str_flag)
```
Thus providing a input of `win` will call this function and return the required flag in hexadecimal format
```
devarjya27@devarjya27-VirtualBox:~$ nc saturn.picoctf.net 61555
Try entering "getRandomNumber" without the double quotes...
==> win
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x63 0x65 0x34 0x62 0x35 0x64 0x35 0x62 0x7d
```
As expected we get our flag. Now converting this to `UTF-16 big endian` we get our flag

![image](https://github.com/user-attachments/assets/5aca7fef-976f-40ce-8c85-f23e128002a6)

## Incorrect Tangents I Went On
None

## What I Learned
Source code if provided is very helpful if we can understand what the program does.

# Picker II

**Flag:** picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_95d44590}

## My Solve
We are provided with the same source code as `Picker I` but we have an additional function `filter()` which returns `False` if our input contains `win`. After struggling for a bit and reading the hint `
Can you do what win does with your input` it clicked to me what if I just printed `flag.txt` as `win` was doing this.
```
devarjya27@devarjya27-VirtualBox:~$ nc saturn.picoctf.net 61764
==> print(open('flag.txt', 'r').read())
picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_95d44590}
'NoneType' object is not callable
```
And we get our flag.

## Incorrect Tangents I Went On
Initially tried to bypass the `filter` function by modifying my input. Tried escape characters, non string input, etc but nothing seemed to work.

## What I Learned
Got better at analyzing source code for programs and finding vulnerabilities.

# Picker III
**Flag:** picoCTF{7h15_15_wh47_w3_g37_w17h_u53r5_1n_ch4rg3_c20f5222}

## My Solve
We can see that the what `call_func` does is calls the function named after the variable depending upon the index in `func_table`. So what  I did was override one of these variables in `func_table` with `win`.
```
devarjya27@devarjya27-VirtualBox:~$ nc saturn.picoctf.net 63682
==> 3
Please enter variable name to write: print_table
Please enter new value of variable: win
==> 1
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x63 0x32 0x30 0x66 0x35 0x32 0x32 0x32 0x7d
```
Here i overrode `print_table` which was stored in index `1` so now when I type `1` instead of `print_table` `win` gets called.

Now converting the hexadecimal version of our flag to `UTF-16 big endian` we get our flag.

Image

## Incorrect Tangents I Went On
Firstly I tried overriding `FUNC_TABLE_SIZE` and `func_table` which clearly was not working after that instead of calling `win` I tried printing `flag.txt` directly as I did in `Picker II` but that also was not working.

## What I Learned
got better at analyzing source code.
