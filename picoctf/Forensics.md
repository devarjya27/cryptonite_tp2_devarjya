# tunn3l_v1s10n
**Flag:** picoCTF{qu1t3_a_v13w_2020}

## My Solve
* In the challenge we asked to download a file and whenever I see the category `forensics` my first instinct is to go to `Hexedit` and look at its contents. So after opening we can see that the file begins with `BM` and after searching for this file signature on google i found out that it is a bitmap image file.

  ![image](https://github.com/user-attachments/assets/2aa61aff-a96a-4b75-ba06-cfe1611af6d2)

* To confirm i even viewed its meta data and sure enough it is a bitmap file.

  ![image](https://github.com/user-attachments/assets/14f31c2d-d7ce-45e1-b52e-bf12a034b7ba)

* Now i tried changing its extension to `bmp` but it was not opening and i was getting the error "unsupported header size"

  ![image](https://github.com/user-attachments/assets/f596bafa-7195-49cc-abc1-b34c162f20df)

* I even tried installing `GIMP` to try and view the file but I was getting the same error.
  ```
  sudo apt install gimp
  ```
  ![image](https://github.com/user-attachments/assets/2877576b-e411-4f19-af94-243b5db514fb)

* So I again opened it in `Hexedit` and I downloaded some sample bitmap image files from the internet and i started comparing them with the challenge file.

* The first difference i found in the header was how the 2 sample images i downloaded had `8A 00 00 00 7C 00` instead of `BA D0 00 00 BA D0` which was in the challenge file.

  ![image](https://github.com/user-attachments/assets/f662887d-5a60-4fcc-b80f-eeaf20de7ae8)   ![image](https://github.com/user-attachments/assets/1591323f-7ec4-4ca3-bf3a-6289d132b311)  

* So I changed that and got the following image

  ![image](https://github.com/user-attachments/assets/e07fd397-2a7d-4fd1-9dfe-8690c3c07d9d)


  ![image](https://github.com/user-attachments/assets/8db8705e-efd7-4d73-a13f-21694463faf8)

* I felt like I was on the correct path so I tried finding more differences from the sample bitmap files and the challenge file.
* I tried more variations, but was not getting anywhere. So i looked up what is the actual meaning of the header in a bitmap image. And i got this on wikipedia.

  ![image](https://github.com/user-attachments/assets/f538aa46-ece8-44df-ad61-15a7d8b6fd21)

* The image after changing the hexdump seemed very small for `2.9 MB` so i thought of maybe increasing the width or height of the image will uncover the entire image and we might get our flag.
* I started off by increasing the height, which according to wikipedia is controlled by 2 bytes at an offset of 22. So I changed `32 01` to `40 04`.

  ![image](https://github.com/user-attachments/assets/b160d810-b89c-4830-a360-af6ca9e0bef7)


  ![image](https://github.com/user-attachments/assets/612674b7-46db-4e87-89c3-64988772a3a2)

* And voila! we get our flag.

## Incorrect Tangents I Went On
* Initially I tried to find the strings present in the file. Grepped it with the keywords "pico" and "CTF" but did not get anything promising.
* Also apparently I installed GIMP for nothing as after changing the headers the file was opening just fine, and there was no need to rename it to `.bmp`.

## What I Learned
* Became more comfortable with `Hexedit`
* Found out what each hex value in the header for `bmp` files mean.

## References
[Wikipedia page for BMP file format](https://en.wikipedia.org/wiki/BMP_file_format)


# extensions

**Flag:** picoCTF{now_you_know_about_extensions}

## My Solve
* Firstly downloading the file, we can see that it is a `.txt` file. After opening it we can figure out that it is a hex dump of a png file as the header begins with `PNG`.

  ![image](https://github.com/user-attachments/assets/df5ad274-6ad3-4205-8183-0dd6965841d6)

* So now saving the file as a png should give us the file. We can save the file as a png by clicking on `Save linked content as` and modify it to a png accordingly

  ![image](https://github.com/user-attachments/assets/bc8771da-74f5-4ab8-9f5c-4a21b13c8a4b)

* Now opening the file, we get the following image:

  ![image](https://github.com/user-attachments/assets/0bc48209-7c38-4df8-b965-ddc774b007f9)

## Incorrect Tangents I Went On
* None

## What I Learned
* While saving a image we can edit its extension when and if required accordingly


# So Meta

**Flag:** picoCTF{s0_m3ta_fec06741}

## My Solve
* Looking at the name of the challenge and the hints it is clear that we need to look at the metadata of the file to get our flag.
* So all I did was put the image in a online metadata viewer and as expected we get our flag.

  ![image](https://github.com/user-attachments/assets/dbc89391-de27-471e-8c51-ae41bb180bdd)

## Incorrect Tangents I Went On
* None

## What I Learned
* Looking up the metadata of a file gives us very important info about a file. It describes various attributes of the file such as file type, file size, creation date, artist, etc...

# Glory of The Garden
**Flag:** picoCTF{more_than_m33ts_the_3y3657BaB2C}

## My Solve
Looking at the hint we can see that it mentions `hex editor` hence I opened the file in `hexed.it` and searched for the substring `picoCTF` and we got our required flag.

![image](https://github.com/user-attachments/assets/3b809126-f225-4cc2-9367-b64cb8cedd16)

## Incorrect Tangents I Went On
* None

## What I Learned
* A hexeditor is one of the most useful tools when it comes to Forensics challenges (saying from experience). Thus one of the first things I should do is open the file in a hex ediot if the file is a image and is under the forensics category.

# PcapPoisoning

**Flag:** picoCTF{P64P_4N4L7S1S_SU55355FUL_fc4e803f}

## My Solve
I opened the pcap file in Wireshark.And the first thing i tried was searching for the substring `picoCTF`by setting a display filter of `tcp contains picoCTF` and thus we got our flag.

![image](https://github.com/user-attachments/assets/443ce065-88fa-4ab8-bf28-d6e6d35da5f1)  


## Incorrect Tangents I Went On
Initially as I did not know how to use `Wireshark` i was using the display filter incorrectly so I watched a yt video on how to use wireshark.

## What I Learned
Got introduced to wireshark and learned how to use it on a basic level.

## References
[Learn Wireshark in 10 minutes - Wireshark Tutorial for Beginners](https://www.youtube.com/watch?v=lb1Dw0elw0Q)

# Wireshark doo dooo do doo...
**Flag:** picoCTF{p33kab00_1_s33_u_deadbeef}

## My Solve
We open up the file in Wireshark and then in the analyze section we can see that we have only one option of following the TCP stream. We scroll through the streams and then on the 5th stream we find something which looks like a encrypted flag.

![image](https://github.com/user-attachments/assets/a49f2c1c-bf10-4c1e-8738-2d50c676e8b0)


Now decoding this with `ROT13` we get our required flag.

![image](https://github.com/user-attachments/assets/c2d5d258-4587-4ef6-9863-784d87a54cde)

## Incorrect Tangents I Went On
Initially i tried to search for `picoCTF` by doing `tcp contains picoCTF` also did that for `http` packets but it didnt work then i followed the tcp stream.

## What I Learned
The `follow` feature analyzes specific streams of data and helps us reconstruct a session for a selected protocol.

# shark on wire 1 
**Flag:** picoCTF{StaT31355_636f6e6e}

## My Solve
Opened the file in `Wireshark` and navigated to `Analyze`->`Follow`->`UDP Stream` as it was the only option/stream to follow. Scrolling through the streams we find our flag at the sixth stream

![image](https://github.com/user-attachments/assets/391b8723-d806-4057-85ed-097425cf84c7)


## Incorrect Tangents I Went On
Initially tried adding a display filter of `udp contains picoCTF` but did not get the flag.

## What I Learned
Became a bit more familiar with `Wireshark`


# Trivial Flag Transfer Protocol
**Flag:** picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

## My Solve
Opened the file in `Wireshark`. Looked at the statistics of the file and saw that two protocols are relevant to us `UDP` and `TFTP`. Navigated to `Analyze`->`Follow` and saw that we are able to follow only the `UDP` stream. Scrolling through the streams it seems that there are some files hidden within the `pcap` file.

![image](https://github.com/user-attachments/assets/61c7b59c-d557-4cb9-8258-3041166285df)  ![image](https://github.com/user-attachments/assets/2a431efd-b6d5-44b7-964c-039f9fbd06e5)


So navigated to the `File` tab and saved the files through `Export Objects`

![image](https://github.com/user-attachments/assets/ce5051ce-8534-4b55-8bac-e431a4d58756) ![image](https://github.com/user-attachments/assets/054653e6-a2b6-4040-a526-858550f846a9)


Now opening the `txt` files we see some sort of encrypted text which we need to somehow decode to move forward. Now opening the `debian package` we see that there is a file called `control`. We open `control` and see that `steghide` was probably used to hide data within the images we exported. 

![image](https://github.com/user-attachments/assets/fda41723-4b70-487d-bb78-54b188118491)

![image](https://github.com/user-attachments/assets/030ddff4-b113-47bb-9d3b-da81585840f0)

![image](https://github.com/user-attachments/assets/6ba1c5de-294b-4350-b7da-ed70131c2a8b)


Now after some trial and error I found out that the text is encrypted by `ROT13`. Decoding `instructions.txt` we see that it says to check `plan`. Now decoding `plan.txt` we can see that it says that it has hid data within the images (probably using steghide) with `DUEDILIGENCE`. After a lot of trial and error it seems that `DUEDILIGENCE` was the passphrase used to hide the data. So we extract the data from the images using `steghide` and with the passphrase `DUEDILIGENCE`

![image](https://github.com/user-attachments/assets/4b736b7e-ccd6-4915-aecd-ffa32906aac9)

![image](https://github.com/user-attachments/assets/5201d78c-fee8-4a14-ab90-ac8b49a53eec)

From the manpage of `steghide` we can see that we can extract data: 
`To extract embedded data from stg.jpg: steghide extract -sf stg.jpg`

```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2/tftp$ steghide extract -sf picture1.bmp
Enter passphrase: 
steghide: could not extract any data with that passphrase!
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2/tftp$ steghide extract -sf picture2.bmp
Enter passphrase: 
steghide: could not extract any data with that passphrase!
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2/tftp$ steghide extract -sf picture3.bmp
Enter passphrase: 
wrote extracted data to "flag.txt".
```

We can see that the data extracted from `picture3.bmp` is saved to `flag.txt`. Opening `flag.txt` we get our required flag.

![image](https://github.com/user-attachments/assets/e0d9fb38-3e47-4520-847b-7fd868f5ba5d)


## Incorrect Tangents I Went On
+ Initially tried adding display filters `udp contains picoCTF` and `tftp contains picoCTF` which did not work.
+ Also navigated through all of the `UDP` streams as i did not know we could export files. So wasted time there.
+ Was initially trying to extract data from the images without any passphrase and after A LOOOOT of time i found out that `DUEDILIGENCE` was actually the passphrase.
+ Had to install `steghide` as i initially did not have it so wasted a lot of time on online steg tools.

## What I Learned
+ Became a lot more familiar with `Wireshark` and `Steghide`. 
+ `tftp` doesnt encrypt the data that is sent or recieved thus we need to hide data in `cheeky` and `cute` ways to protect our data (if we use `trivial file transfer protocol`).

# WebNet0

**Flag:** picoCTF{nongshim.shrimp.crackers}

## My Solve
Loooking at the hint `How can you decrypt the TLS stream?` it was clear that adding simple display filters wont work as it previously worked for other challenges. So I looked into `TLS` streams and from what i understand is that communications via `TLS` are known as `TLS handshakes` and these `handshakes` are encrypted data which can only be decoded with their corresponding keys. That is why the challenge provided us with a packet capture and with a key.

ALso found out that we can import keys into `Wireshark` and it will automatically decrypt the traffic.

So i uploaded the key firstly by navigating to `Edit`-> `Preferences` -> `Protocols` -> `TLS` and clicking `Edit`.

![image](https://github.com/user-attachments/assets/25c8373d-a532-4079-8948-83c3a8076d79)

Now opening the `packet capture` in `Wireshark` we get decrypted traffic. So all i did was follow the `TLS` stream and got the required flag.

![image](https://github.com/user-attachments/assets/b95888fa-a2e2-400b-b353-94a0ef2d0aee)

## Incorrect Tangents I Went On
As initially i did not we could upload keys into wireshark, wasted a lot of time doing random stuff like following the TCP stream and so on.

## What I Learned
Learned about `TLS` streams and how to upload keys to wireshark so as to help with decrypting the traffic. Also learned to look at hints on picoCTF so that i do not waste time doing stupid stuff.

# m00nwalk
**Flag:** picoCTF{beep_boop_im_in_space}
## My Solve
Looking at the hint `How did pictures from the moon landing get sent back to Earth?` I asked chatgpt the same question and it said that they were transfered via `SSTV`.

![image](https://github.com/user-attachments/assets/82548624-93e9-44c1-a4d6-336becd06628)

So I searched for `sstv decoder` and came across [this](https://github.com/colaclanth/sstv). Followed the installation instructions which were given in the `README` of the repo
```
$ git clone https://github.com/colaclanth/sstv.git
```
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ git clone https://github.com/colaclanth/sstv.git
Cloning into 'sstv'...
remote: Enumerating objects: 221, done.
remote: Counting objects: 100% (59/59), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 221 (delta 51), reused 49 (delta 49), pack-reused 162 (from 1)
Receiving objects: 100% (221/221), 1.01 MiB | 3.46 MiB/s, done.
Resolving deltas: 100% (139/139), done.
```
After this used the decoder according to the sample given in `Usage`:
```
$ sstv -d audio_file.wav -o result.png
```
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ sstv -d message.wav -o result.png
[sstv] Searching for calibration header... Found!    
[sstv] Detected SSTV mode Scottie 1
[sstv] Decoding image...   [#############################################] 100%
[sstv] Drawing image data...
[sstv] ...Done!
```
Now opening `result.png` we get our flag.

![image](https://github.com/user-attachments/assets/5e0a8fc6-4462-4d3b-a7b6-3f7dd1b9c239)

## Incorrect Tangents I Went On
Opened up an [online sstv decoder](https://ckegel.github.io/Web-SSTV/decode.html) which was not working. Then I found the github repo which was bestowed upon us by God himself.

## What I Learned
learnt how to use a sstv decoder made by `colaclanth`

## Resources
[sstv decoder](https://github.com/colaclanth/sstv)

# Packets Primer
**Flag:**picoCTF{p4ck37_5h4rk_ceccaa7f}

# My Solve
We are given a packet capture file so the first thing which came to mind was to open it in `wireshark` so i did that and opening the `statistics` of the file we can see that the only evident protocol is `TCP`.

Now navigating to the `Analyze` Tab and selecting `Follow` and then `TCP Stream` we get our flag at stream 0.

## Incorrect Tangents I Went On
None as it was a pretty straightforward challenge

## What I Learned
Becam more familiar with `Wireshark`.

# Matroyshka doll
**Flag:**picoCTF{e3f378fe6c1ea7f6bc5ac2c3d6801c1f}

Looking at the hint `Wait, you can hide files inside files? But how do you find them?` I tried searching for a tool that can help with extracting data from files and apparently `binwalk` is a popular tool for such ctf challenges. So I installed `binwalk` on my machine and looking at its `manpage` the argument `-B` scans the file for signatures whereas `--extract` extracts all possible data that can be extracted.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ binwalk -B dolls.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378954, uncompressed size: 383940, name: base_images/2_c.jpg
651612        0x9F15C         End of Zip archive, footer length: 22

```
Here we can see that there is a image `2_c.jpg` hidden in this file. So I extracted it.
```devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ binwalk --extract dolls.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378954, uncompressed size: 383940, name: base_images/2_c.jpg
651612        0x9F15C         End of Zip archive, footer length: 22
```
Now we exract `2_c.jpg`:
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2/_dolls.jpg.extracted/base_images$ binwalk --extract 2_c.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 526 x 1106, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
187707        0x2DD3B         Zip archive data, at least v2.0 to extract, compressed size: 196045, uncompressed size: 201447, name: base_images/3_c.jpg
383807        0x5DB3F         End of Zip archive, footer length: 22
383918        0x5DBAE         End of Zip archive, footer length: 22
```
We get `3_c.jpg` which we again extract:
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted/base_images$ binwalk --extract 3_c.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 428 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
123606        0x1E2D6         Zip archive data, at least v2.0 to extract, compressed size: 77653, uncompressed size: 79808, name: base_images/4_c.jpg
201425        0x312D1         End of Zip archive, footer length: 22
```
In the same way we extract `4_c.jpg`
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted/base_images$ binwalk --extract 4_c.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 320 x 768, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
79578         0x136DA         Zip archive data, at least v2.0 to extract, compressed size: 64, uncompressed size: 81, name: flag.txt
79786         0x137AA         End of Zip archive, footer length: 22
```
Now we get `flag.txt` which contains the flag to our challenge.

## Incorrect Tangents I Went On
+ Tried opening it in a `hexeditor` to look for some additional info but then found out about `binwalk` which led me to the solution.

## What I Learned
+ Learnt to use `binwalk` which seems to be a very important tool for CTFs in general.

# like1000

**Flag:** picoCTF{l0t5_0f_TAR5}

## My Solve
We are given a `.tar` archive named `1000.tar` for the challenge and extracting it we get `999.tar`. Now looking at the hint it is evident that we need to create a script to extract the file 1000 times as doing so manually is impossible. Following is my script:
```
import os 
for i in range(1000, 0, -1):
    os.system(f"tar -xf {i}.tar")
    
```
Now upon extracting the last `1.tar` file we get a image that contains our flag.

## What I Learned
Learned to extract `.tar` files and got a bit of practice with scripting as I have a very bad habit of doing stuff manually.

## Incorrect Tangents I Went On
None

# Sleuthkit Intro
**Flag:** picoCTF{mm15_f7w!}

## My Solve
The challenge is very straightforward, we are given a disk image and asked to run `mmls` on it to get the size of the linux partition.

Downloading the image and running `mmls` on it we get the following output:
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ mmls disk.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)
```

We can see that the length of the linux partition is `202752` sectors. So now all we need to do is connect to the remote server and enter our answer
```
taci-picoctf@webshell:~$ nc saturn.picoctf.net 62744
What is the size of the Linux partition in the given disk image?
Length in sectors: 202752
202752
Great work!
picoCTF{mm15_f7w!}
```
For some reason `netcat` was not working so I had to use the browser webshell available on `picoctf`'s site.

## What I Learned
Learnt what the `mmls` command does.

## Incorrect Tangents I Went On
None

# Sleuthkit Apprentice
**Flag:** picoCTF{by73_5urf3r_2f22df38}

## My Solve
This challenge racked my brain quite a bit as I had to learn about Sleuthkit commands and how to use them.

Firstly downloaded and extracted our img file then ran `mmls` on it.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ mmls disk.flag.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000360447   0000153600   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)
```
Here we can see that there are 2 Linux systems which are of interest to us and should contain our flag. So i made 2 differrent partitioned image files (with the help of chatGPT) of these Linux systems which we can use individually for further analysis.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ dd if=disk.flag.img of=partition1.img bs=512 skip=2048 count=204800
204800+0 records in
204800+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 2.2357 s, 46.9 MB/s
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ dd if=disk.flag.img of=partition2.img bs=512 skip=360448 count=253952
253952+0 records in
253952+0 records out
130023424 bytes (130 MB, 124 MiB) copied, 2.63272 s, 49.4 MB/s
```

Now we list the file and directory information of `partition1.img` using `fls`.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ fls partition1.img
d/d 11:	lost+found
r/r 12:	ldlinux.sys
r/r 13:	ldlinux.c32
r/r 15:	config-virt
r/r 16:	vmlinuz-virt
r/r 17:	initramfs-virt
l/l 18:	boot
r/r 20:	libutil.c32
r/r 19:	extlinux.conf
r/r 21:	libcom32.c32
r/r 22:	mboot.c32
r/r 23:	menu.c32
r/r 14:	System.map-virt
r/r 24:	vesamenu.c32
V/V 25585:	$OrphanFiles
```
Nothing much that seems to relate to `flag`. Now running `fls` on `partition2.img` we get a bunch of files and directories. So i grepped it with `flag`.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ fls -r partition2.img | grep flag
++ r/r * 2082(realloc):	flag.txt
++ r/r 2371:	flag.uni.txt
```

We get 2 promising files. Now we use `icat` to read these files as we know their `inode` numbers.
```
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ icat partition2.img 2082
            3.449677            13.056403
devarjya27@devarjya27-VirtualBox:~/Cryptonite TP2$ icat partition2.img 2371
picoCTF{by73_5urf3r_2f22df38}
```
And voila `flag.uni.txt` contained our flag.

## What I Learned
Learnt a lot about sleuthkit commands especially `mmls`, `fls` and `icat`. Learnt how to make partitions of a disk using `dd`.

## Incorrect Tangents I Went On
+ Initially did not do `mmls` and went directly to doing `fls` and `icat` which was not working as it was not recognizing the type of file the original `img` file was.

## References
[Sleuthkit commands wiki page](https://wiki.sleuthkit.org/index.php?title=The_Sleuth_Kit_commands)

[icat manpage](https://www.sleuthkit.org/sleuthkit/man/icat.html)

# Secret of The Polyglot
**Flag:** picoCTF{f1u3n7_1n_pn9_&_pdf_724b1287}

## My Solve
We are given a `pdf` file which contains the second part of the rewuired flag. 

![image](https://github.com/user-attachments/assets/459e7c97-7f6e-4853-b96d-3f7f84f2c579)


Just to be sure i ran the `file` command and what i got back was very interesting
```
devarjya27@devarjya27-VirtualBox:~/Downloads$ file flag2of2-final.pdf
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```
The `pdf` file is actually a `png` image. Changing the file extension to `png` we get the first part of our flag

![image](https://github.com/user-attachments/assets/30f348f4-adc4-402d-a532-15da805e394a)

## What I Learned
Nothing extra really as it was a very simple challenge

## Incorrect Tangents I Went On
None
