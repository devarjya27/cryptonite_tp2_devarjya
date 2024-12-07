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

Image Image

So navigated to the `File` tab and saved the files through `Export Objects`

Image Image

Now opening the `txt` files we see some sort of encrypted text which we need to somehow decode to move forward. Now opening the `debian package` we see that there is a file called `control`. We open `control` and see that `steghide` was probably used to hide data within the images we exported. 

Image

Now after some trial and error I found out that the text is encrypted by `ROT13`. Decoding `instructions.txt` we see that it says to check `plan`. Now decoding `plan.txt` we can see that it says that it has hid data within the images (probably using steghide) with `DUEDILIGENCE`. After a lot of trial and error it seems that `DUEDILIGENCE` was the passphrase used to hide the data. So we extract the data from the images using `steghide` and with the passphrase `DUEDILIGENCE`

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

Image

## Incorrect Tangents I Went On
+ Initially tried adding display filters `udp contains picoCTF` and `tftp contains picoCTF` which did not work.
+ Also navigated through all of the `UDP` streams as i did not know we could export files. So wasted time there.
+ Was initially trying to extract data from the images without any passphrase and after A LOOOOT of time i found out that `DUEDILIGENCE` was actually the passphrase.
+ Had to install `steghide` as i initially did not have it so wasted a lot of time on online steg tools.

## What I Learned
+ Became a lot more familiar with `Wireshark` and `Steghide`. 
+ `tftp` doesnt encrypt the data that is sent or recieved thus we need to hide data in `cheeky` and `cute` ways to protect our data (if we use `trivial file transfer protocol`).
