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
<https://en.wikipedia.org/wiki/BMP_file_format>


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
