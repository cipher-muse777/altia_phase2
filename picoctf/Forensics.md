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
  with open("genuienlylosingitnow.bmp", "rb") as f:
  
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
```
- so i got this image and therfore the flag
<img width="895" height="658" alt="Screenshot 2025-10-26 104334" src="https://github.com/user-attachments/assets/405f5323-0200-42cd-82a1-b5b1d5c72d79" />

  

