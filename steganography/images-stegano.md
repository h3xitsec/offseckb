## Python script to XOR image file
```python
from PIL import Image
from pwn import *

def main():

    origin = Image.open("key.png")
    newImage = Image.open("image.png")

    leak_bytes = xor(origin.tobytes(), newImage.tobytes())
    leak = Image.frombytes(newImage.mode, newImage.size, leak_bytes)

    #leak.save("image.png", format="png")
    leak.show()

if __name__ == '__main__':
    main()
	```