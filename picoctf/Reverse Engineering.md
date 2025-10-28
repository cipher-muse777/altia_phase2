# 3.VAULT DOOR 3
This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## SOLUTION 
```
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
        }
    }

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm18gb41_u_4_mfr340");
    }
}

```
- this was the given java file which when put into a java compiler asks for a password
- while analysing the code i understood the password is picoCTF{...}
- to decode the words needed inside the curly brackets the last few lines of code is the key with respect to jU5t_a_sna_3lpm18gb41_u_4_mfr340
- the first 8 characters were copied as such
- the next characters till 15 is in reverse order from 23rd to 8th
- for even positions from 16th to 30th, we must take the input in reversed order
- for odd positions we take the characters in the same position
- we can make a python file to do this task
  FINAL CODE
  ```
  def decode_vault_password():
    target = "jU5t_a_sna_3lpm18gb41_u_4_mfr340"
    password = [''] * 32
    
    print("Step-by-step decoding:")
    print("=" * 50)
    
    # Step 1: Reverse the first stage (direct copy)
    print("Step 1: Reverse buffer[0-7] = password[0-7]")
    for i in range(8):
        password[i] = target[i]
        print(f"  password[{i}] = target[{i}] = '{target[i]}'")
    
    # Step 2: Reverse the second stage
    print("\nStep 2: Reverse buffer[8-15] = password[23-i]")
    for i in range(8, 16):
        password_index = 23 - i
        password[password_index] = target[i]
        print(f"  password[{password_index}] = target[{i}] = '{target[i]}'")
    
    # Step 3: Reverse the third stage
    print("\nStep 3: Reverse buffer[16,18,20,22,24,26,28,30] = password[46-i]")
    for i in range(16, 32, 2):
        password_index = 46 - i
        password[password_index] = target[i]
        print(f"  password[{password_index}] = target[{i}] = '{target[i]}'")
    
    # Step 4: Reverse the fourth stage
    print("\nStep 4: Reverse buffer[31,29,27,25,23,21,19,17] = password[i]")
    for i in range(31, 16, -2):
        password[i] = target[i]
        print(f"  password[{i}] = target[{i}] = '{target[i]}'")
    
    # Build the final password
    final_password = ''.join(password)
    
    print("\n" + "=" * 50)
    print(f"Decoded password: {final_password}")
    print(f"Flag: picoCTF{{{final_password}}}")
    
    # Verify the result
    print("\nVerification:")
    if verify_password(final_password, target):
        print("assword successfully verified!")
    else:
        print("Verification failed!")
    
    return final_password

  def verify_password(password, target):
    if len(password) != 32:
        return False
    
    buffer = [''] * 32
    
    # Apply the original transformation
    for i in range(8):
        buffer[i] = password[i]
    
    for i in range(8, 16):
        buffer[i] = password[23 - i]
    
    for i in range(16, 32, 2):
        buffer[i] = password[46 - i]
    
    for i in range(31, 16, -2):
        buffer[i] = password[i]
    
    result = ''.join(buffer)
    return result == target
    
    def quick_decode():
    target = "jU5t_a_sna_3lpm18gb41_u_4_mfr340"
    password = [''] * 32
    
    # Stage 1 reverse
    for i in range(8):
        password[i] = target[i]
    
    # Stage 2 reverse
    for i in range(8, 16):
        password[23 - i] = target[i]
    
    # Stage 3 reverse
    for i in range(16, 32, 2):
        password[46 - i] = target[i]
    
    # Stage 4 reverse
    for i in range(31, 16, -2):
        password[i] = target[i]
    
    return ''.join(password)
    print("Detailed Decoding Process:")
    result = decode_vault_password()
    
    print("\n" + "=" * 50)
    print("Quick Solution:")
    quick_result = quick_decode()
    print(f"Password: {quick_result}")
    print(f"Flag: picoCTF{{{quick_result}}}")
```
```
- hence obtainig the flag picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}

## FLAG 
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}

## CONCEPTS LEARNED 
- to properly read java code
- to use that knowledge to form python code to decode

## RESOURCES 
https://www.geeksforgeeks.org/software-engineering/software-engineering-reverse-engineering/
https://www.w3schools.com/python/python_ref_string.asp?utm_source=chatgpt.com
https://www.w3schools.com/java/
