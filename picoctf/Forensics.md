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

