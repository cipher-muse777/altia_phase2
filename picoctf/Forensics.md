# 1.TRIVIAL FLAG TRANSFER PROTOCOL 
Figure out how they moved the flag.

## SOLUTION 
- after opening the file using wireshark (4.4.2 version), i observed the given texts.
- under the instruction.txt there was capital letters which looked important
- so putting it into a cipher identifier i understood its a ROT13 cipher
- after decoding i got the message TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER. FIGURE OUT A WAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN.
<img width="1917" height="1131" alt="Screenshot 2025-10-29 162145" src="https://github.com/user-attachments/assets/59352b77-e783-4ccf-aa8f-36ac9e642d75" />

- i scrolled a bit more and found an another string of similar capital letters which then decoded into I USED THE PROGRAM AND HID IT WITH -DUEDILIGENCE. CHECK OUT THE PHOTOS
  <img width="1919" height="1125" alt="Screenshot 2025-10-29 162203" src="https://github.com/user-attachments/assets/87f6e4f8-b4e0-4abe-8f1c-3e3f6424ac46" />

- to find these photos i went to export objects and then selected tftp and saved them all
 <img width="1118" height="807" alt="image" src="https://github.com/user-attachments/assets/bba0c6df-bd84-41f0-ae12-f64ab879b678" />
 

 - i uploaded each of those images into a steganographic decoder and put the password DUEDILIGENCE and finally the third picture gave the flag
   <img width="1919" height="325" alt="Screenshot 2025-10-29 162643" src="https://github.com/user-attachments/assets/debee315-2668-457c-8644-451b1099f408" />


# 2.tunn3l v1s10n
We found this file. Recover the flag.

## SOLUTION 
- we first open the given file using hexeditor and while observing the header 42 4D, it was revealed that its a bmp file
- but when we try to open using an online bmp file viewer it doesnt work showing

<img width="1632" height="101" alt="Screenshot 2025-10-25 165831" src="https://github.com/user-attachments/assets/7a4835f5-69cf-4c23-934f-6b5e9dd3d925" />

- so we might be able to extract using a python file 
  
```
from PIL import Image
import io


with open("tunn3l v1s10n.bmp", "rb") as f:
    data = f.read()


pixel_data = data[53434:]


width = 1134
height = 500
mode = "RGB"


row_size = (width * 3 + 3) & ~3  
expected_size = row_size * height


if len(pixel_data) < expected_size:
    raise ValueError("Pixel data is too short — header may be misaligned or corrupted.")


pixel_data = pixel_data[:expected_size]


img = Image.frombuffer(mode, (width, height), pixel_data, "raw", mode, row_size, -1)
img.show()

print("Available pixel data:", len(pixel_data))
print("Expected pixel data:", expected_size)
```
- i got the image 
<img width="1667" height="451" alt="Screenshot 2025-10-25 173238" src="https://github.com/user-attachments/assets/929832c1-373c-454f-aa07-4d0ccb11bd9f" />
- since that was the fake, we could calculate how many rows of pixel data are available and then rebuild an image using all pixel data

  FINAL CODE
  
  ```
  from PIL import Image
  with open("tunn3l v1s10n.bmp", "rb") as f:
  
    data = f.read()
  pixel_data = data[53434:]
  width = 1134
  mode = "RGB"
  row_size = (width * 3 + 3) & ~3

  full_height = len(pixel_data) // row_size
  full_expected_size = row_size * full_height
  full_data = pixel_data[:full_expected_size]
  full_img = Image.frombuffer(mode, (width, full_height), full_data, "raw", mode, row_size, -1)
  full_img.show()

  print("Full image height:", full_height)
  print("Full expected pixel data:", full_expected_size)
  print("Available pixel data:", len(pixel_data))
  ```

- so i got this image and therfore the flag
<img width="895" height="658" alt="Screenshot 2025-10-26 104334" src="https://github.com/user-attachments/assets/405f5323-0200-42cd-82a1-b5b1d5c72d79" />

## FLAG
picoCTF{qu1t3_a_v13w_2020}

## CONCEPTS LEARNED 
- how analysing the hex editor helps us identify the type of file given
- how row alignment and padding in BMP files is necessary; BMP enforces that each row's byte size is a multiple of 4 bytes, which may add extra padding
- how calculating the expected number of rows and image dimensions from byte counts, width, and padding really helps us recover misalligned files
- how color data =is BGR order for BMP, which we then convert to rgb while processing the image

## RESOURCES 
https://en.wikipedia.org/wiki/BMP_file_format
https://stackoverflow.com/questions/10439104/reading-bmp-files-in-python
https://www.youtube.com/watch?v=VBY_UEiw9F8

# 3.m00nwalk
Decode this message from the moon.

## SOLUTION 
- there were two given hints 1.How did pictures from the moon landing get sent back to Earth? and 2.What is the CMU mascot?, that might help select a RX option
- so i searched more about how the pictures from moon was sent back to earth and realised it was using radio waves and were transmitted using a specialized Slow Scan Television (SSTV) system
- so i used an online sstv decoder https://sstv-decoder.mathieurenaud.fr/ to decode the audio and got the flag
  <img width="320" height="256" alt="download" src="https://github.com/user-attachments/assets/3ebf96d8-48b1-4451-b3c7-6931b2ebda61" />

## FLAG
picoCTF{beep_boop_im_in_space}

## CONCEPTS LEARNED 
- how radiowaves helps us transmit images in form of waves
- Slow Scan Television (SSTV) is a technique that uses radio waves to transmit still images slowly with low bandwidth
- how the first images from the Apollo Moon landing were transmitted using a specialized form of SSTV

## RESOURCES
https://www.scopeofwork.net/how-slow-scan-tv-shaped-the-moon/
https://sstv-decoder.mathieurenaud.fr/
  

