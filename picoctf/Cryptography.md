# C3
**Flag:** picoCTF{adlibs}

## My Solve
* In this challenge we are given a encyption code and a ciphertest which we need to decrypt. Looking at the code we can see that the characters in `lookup1` maps to characters in `lookup2` in a special way.
```
import sys
chars = ""
from fileinput import input
for line in input():
  chars += line

lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""

prev = 0
for char in chars:
  cur = lookup1.index(char)
  out += lookup2[(cur - prev) % 40]
  prev = cur

sys.stdout.write(out)
```
* `cur` gets the index of the character in `lookup1` and then `cur - prev` is calculated with `prev` initially set to 0, then it is modded by 40 so that it does not exceed the number of characters stored i.e. 40. this shift is used as the index in `lookup2` to which the initial character in `lookup1` maps to. After that `prev`is set to `cur` after which the encryption process starts again for a the next character.

* To decrypt the message all I had to do was replace `lookup1` by `lookup2` and vice versa and change `cur-prev` to `cur+prev`. After that setting `prev` to `original` instead of `cur` which will give us the original index. This will reverse the encryption process and return the decrypted message.

```
chars = ""
lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""

prev = 0
for char in chars:
  cur = lookup2.index(char)
  original = (cur + prev) % 40
  out += lookup1[original]
  prev = original

print(out)
```
* In place of `chars` i put in the ciphertext and i got the following output. The decrypted message is saved to `decoded.txt`.

![image](https://github.com/user-attachments/assets/276aa5e1-178b-4131-ac4f-c57bfe86395c)


The decrypted message is another piece of code which now returns specific characters from `chars` derived from a given function `b*b*b`. Passing `decoded.txt` as its input gives us the contents of the flag.

```
#asciiorder
#fortychars
#selfinput
#pythontwo

chars = ""
from fileinput import input
for line in input():
    chars += line
b = 1 / 1

for i in range(len(chars)):
    if i == b * b * b:
        print (chars[i], end="") #prints
        b += 1 / 1
print()
```

![image](https://github.com/user-attachments/assets/85e38277-99e0-40c7-9a15-d8451609d2c5)


## Incorrect Tangents I Went On
I made a mistake towards the end where i was passing in the original ciphertext in the second script which gave me the wrong flag, after which trying it with the decrypted message gave me the correct flag.

![image](https://github.com/user-attachments/assets/dbddc286-9150-48dc-9bd3-be77b4776379)

## What I Learned
From this challenge i learned to  decode a basic custom cipher. I realized not all ciphers can be solved using online cryptography decoders. At times writing our own script to decode a particular ciphertext may be required.
