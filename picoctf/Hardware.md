# 1. IQ TEST 
let your input x = 30478191278.
wrap your answer with nite{ } for the flag.
As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.

## SOLUTION 
- manually solved the logic gates, where i first converted the number to binary 011100011000101001000100101010101110
![WhatsApp Image 2025-10-30 at 19 16 11](https://github.com/user-attachments/assets/0d790570-e70e-47e8-91e4-b6fdec607b20)

## FLAG 
nite{100010011000}

## CONCEPTS LEARNED 
- logic gates??

## RESOURCES
none?

# 2. I LIKE LOGIC 
i like logic and i like files, apparently, they have something in common, what should my next step be.

## SOLUTION 
- i installed logic2 and then open the sal file to analyse it.
- out of all the channels only channel 3 had a square wave
<img width="1913" height="1094" alt="Screenshot 2025-10-30 192851" src="https://github.com/user-attachments/assets/6f5973e9-3a95-44d2-94f9-58ab269f4c89" />

- so i used async serial and changed the baud for all standard values and finally got it with 9600
- then i changed the hex to ascii
<img width="228" height="622" alt="Screenshot 2025-10-30 193005" src="https://github.com/user-attachments/assets/c1b4037e-18f0-4fc9-9fb1-8aace5214c51" />

- then searched up { to find the flag
<img width="443" height="273" alt="Screenshot 2025-10-30 193103" src="https://github.com/user-attachments/assets/dbe13ea1-4fea-401f-9838-84749e149960" />

<img width="950" height="232" alt="Screenshot 2025-10-30 193126" src="https://github.com/user-attachments/assets/0bbbf5ea-f07c-45f4-8ac8-568ad902d5db" />

## CONCEPTS LEARNED 
- how we can analyse sal files using logic2

## RESOURCES 
https://www.youtube.com/watch?v=Ak9R4yxQPhs&t=1s

## 3. BARE METAL ALCHEMIST 
My friend recommended me this anime but i think i've heard a wrong name.

## SOLUTION 
- first installed avr using sudo apt install binutils-avr avr-libc
- then i gave executable permission using chmod +x ./firmware.elf
- then after that i opened the file using file ./firmware.elf
- so it showed 32 bit elf so i sudo apt install libc6:i386 libstdc++6:i386
- then i sudo apt install simavr simavr -g -m atmega328p ./firmware.elf
  
<img width="1904" height="98" alt="image" src="https://github.com/user-attachments/assets/affaefac-74bb-4db9-a3d7-b8b53135b99b" />

- then i disassembled using avr-objdump -d ./firmware.elf
  
<img width="1919" height="1059" alt="image" src="https://github.com/user-attachments/assets/7e4b8fdd-c2c0-4353-89f9-a1a90a0332dc" />

- then it was observed that there was xor in it

<img width="1010" height="342" alt="Screenshot 2025-10-30 195645" src="https://github.com/user-attachments/assets/dc12f378-2bc7-49db-936d-fbbdd638d973" />

- so we can use a python code to decrypt it

FINAL CODE 
```
encoded_data = [
    0xf1, 0xe3, 0xe6, 0xe6, 0xf1, 0xe3, 0xde, 0xf1, 0xcd, 0x94, 0xd6, 0xfa,
    0x94, 0xd6, 0xfa, 0xd6, 0xca, 0xc8, 0x96, 0xfa, 0xd6, 0x94, 0xc8, 0xd5,
    0xc9, 0x96, 0xfa, 0x91, 0xd7, 0xc1, 0xd0, 0x94, 0xcb, 0xca, 0xfa, 0xc3,
    0x94, 0xd7, 0xc8, 0xd2, 0x91, 0xd7, 0xc0, 0xd8
]

# XOR key is 0xA5
key = 0xA5

decoded = bytes([byte ^ key for byte in encoded_data])
print("Decoded data:", decoded)

try:
    decoded_str = decoded.decode('ascii', errors='replace')
    print("As string:", decoded_str)
except:
    print("Could not decode as ASCII")
    
```

## FLAG 
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}

## CONCEPTS LEARNED
-how to use avr reader, simavr etc etc

## RESOURCES
https://github.com/silragon-ryu/Avr-cheatsheet/blob/main/cheat-sheet.pdf
