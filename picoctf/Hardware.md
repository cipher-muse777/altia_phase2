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

## BARE METAL ALCHEMIST 
My friend recommended me this anime but i think i've heard a wrong name.





