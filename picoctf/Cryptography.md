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

# Custom Encryption
**Flag:** picoCTF{custom_d2cr0pt6d_dc499538}

## My Solve
We are given the encoded flag and the encryption code. Our job is to decode the flag by modifying the code given to us. Firstly lets understand `custom_encryption.py`:
```
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')

if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
```
+ We can see that we input a `message` which gets passed to `test` along with `trudeau`
  ```
  if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
  ```
+ Now in `test` we can see that a and b are random integers generated using `p` and `g` and then through the `generator` function `u`, `v`, `key` and `b_key` are generated and then only if `b_key` and `key` are same then we use it as the `key` for encryption.
  ```
  def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')
  ```
+ Now a `semi_cipher` is formed by passing the `message` and `trudeau` as the `text_key` to `dynamic_xor_encrypt` after which the message is completely encrypted by passing the `semi_cipher` along with `shared_key` which is same as `key` to the `encrypt` function.
+ In `dynamic_xor_encrypt` the `plain_text` is reversed and each character of this string is `XOR`ed with `key_char` (in a cyclical manner) which is the current character of `test_key`. The `XOR` encrypted character is now appended to `cipher_text` and then finally returned.
  ```
  def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text
  ```
+ Now in `encrypt` the ascii value of each character of `plain_text` is multiplied with `key` and with `311` and then returning the final encrypted message.
  ```
  def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher
  ```
Now this was my script `decryption.py` to decrypt the encoded flag:
```
p = 97
g = 31
a = 89
b = 27

cipher = [33588, 276168, 261240, 302292, 343344, 328416, 242580, 85836, 82104, 156744, 0, 309756, 78372, 18660, 253776, 0, 82104, 320952, 3732, 231384, 89568, 100764, 22392, 22392, 63444, 22392, 97032, 190332, 119424, 182868, 97032, 26124, 44784, 63444]

def generator(g, x, p):
    return pow(g, x) % p

def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext):
        key_char = text_key[i % key_length]

        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char

    return cipher_text[::-1]

v = generator(g, b, p)
key = generator(v, a, p)

semi_decrypted=""

for c in cipher:
    semi_decrypted += chr(c // 311 // key)

print(dynamic_xor_encrypt(semi_decrypted, "trudeau"))
```
+ Firstly I initialized `a`, `b`, `p` and `g` along with cipher which are already known to us.
+ Now to reverse the `encrypt` function all i did was instead of now multiplying i divided by key and then by 311.
+ Now digging a bit about `XOR` i found out that `XOR` is a reversible processs which means if we `XOR` A using C as they key we get, lets say B as the output. Now if we `XOR` B with C we should get back A.
+ So i passed in `semi_decrypted` with `trudeau` as the key and we get our required flag. the only adjustment i had to do in `dynamic_xor_encrypt` was that now instead of cycling through the reversed `plain_text` we are now cycling through `plain_text` and instead of returning `cipher_text` we are returning the reverse of `cipher_text`. Basically as we are going in a reverse direction we iterated through `plain_text` and not is reverse.
```
devarjya27@devarjya27-VirtualBox:~$ python -u "/home/devarjya27/Cryptonite TP2/decryption.py"
picoCTF{custom_d2cr0pt6d_dc499538}
```

## Incorrect Tangents I Went On
+ Initially tried to write everything from scratch instead of modifying the code which was already given to us. Wasted time there.
+ Was getting the wrong output as initially iterated through `plain_text[::1]` instead of `plain_text` while trying to decode by XORing. Fixed it accordingly.
+ was initially generating `key` and `b_key` and then checking for if they are same ten assigning to shared key which would have worked but is just redundant as they are the same and finding out only one of them will work just as fine.

## What I Learned
Arguably got better at analyzing codes which custom encrypt messages. Still hate cryptography tho.

# miniRSA
**Flag:** picoCTF{n33d_a_lArg3r_e_d0cd6eae}
## My Solve
initially as i did not know much about RSA i watched a couple of videos (added in the references section) and had chatgpt explain it to me. FOund out that RSA works on the algorithm `c≡M^e (mod N)` where `c` is the ciphertext `M` is the `plaintext`. Looking at the `ciphertext` file it seems that `e` is very small and i found out that if `e` is very small then the algorithm changes to `c≡M^e`. Which means that if we find the cube root of the cipher we get our plaintext number.

The following is my script for decryption using `gmpy2` which made it very easy to find the plaintext number and i did not need to create a different function to calculate the root.
```
import gmpy2

c = 2205316413931134031074603746928247799030155221252519872650080519263755075355825243327515211479747536697517688468095325517209911688684309894900992899707504087647575997847717180766377832435022794675332132906451858990782325436498952049751141 
e = 3      
M = gmpy2.iroot(c, e)[0]  
print(M)

plaintext_number = int(M) 
plaintext_bytes = plaintext_number.to_bytes((plaintext_number.bit_length() + 7) // 8, 'big')
print(plaintext_bytes.decode('utf-8', errors='coerce'))
```
```
[Running] python -u "e:\Programs\decrypt.py"
13016382529449106065894479374027604750406953699090365388203722801043052339225981
picoCTF{n33d_a_lArg3r_e_d0cd6eae}
```
## Incorrect Tangent I Went On
Initially while writing the decryption script, the script was not able to handle such large numbers and i was getting a lot of errors so i used help from chatgpt which suggested me to use `gmpy2` and then helped me to convert the plaintext number to the actual plaintext

## What I Learned
Learned about RSA and also learned that if `e` is very small then it becomes vulnerable to `small exponent attack` which can be taken advantage of and let others read the encrypted data without needing the key to decrypt it

## Refereences
[RSA Cryptosystem](https://en.wikipedia.org/wiki/RSA_(cryptosystem))

[How RSA Encryption Works](https://www.youtube.com/watch?v=ZPXVSJnDA_A)
