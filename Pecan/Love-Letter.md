# Love Letter Challenge Write-Up

Hello cyber people! Today I’m doing a challenge called **Love Letter** on a site called **Pecan** :D. This is mainly a warm-up. The challenge involves a letter for a girlfriend, and there’s a cipher we need to decipher. A Python script is provided, so let’s dive in!

---

## Table of Contents

1. [The Python Script](#the-python-script)  
2. [What the Script Does](#what-the-script-does)  
3. [Extracting the Ciphertext](#extracting-the-ciphertext)  
4. [Decrypting the Message](#decrypting-the-message)  
5. [Converting ASCII to Text](#converting-ascii-to-text)  
6. [Final Message](#final-message)  
7. [Lessons Learned](#lessons-learned)  
8. [References](#references)  

---

## The Python Script

```python
def encryptionFunction():
    # Variables
    ciphertext = []
    ascii_values = []
    
    # Prompt to enter plaintext to be encrypted
    plaintext = input('Please enter text to be encrypted: ')

    # Each letter in the message is converted to ASCII and appended to a list
    for character in plaintext:
        ascii_values.append(ord(character))

    # Each ASCII character is multiplied by 3
    for element in ascii_values:
        ciphertext.append(element * 3)

    # Ciphertext is returned
    return ciphertext

while True:
    print(encryptionFunction())
```

---

## What the Script Does

1. **Prompts for input**: Asks the user to type a message.  
2. **Converts to ASCII**: Transforms each character into its ASCII code via `ord()`.  
3. **Encrypts by multiplication**: Multiplies each ASCII value by 3.  
4. **Outputs ciphertext**: Prints the resulting list of numbers.

---

## Extracting the Ciphertext

When the mega_secure_encryptor.py file runs, it eventually outputs a list like:

```
[315, 324, 363, 294, 294, 363]
```

This is our **ciphertext** and what the challenge wants us to decipher!

---

## Decrypting the Message

Since encryption was done by **multiplying** each ASCII code by 3, we reverse it by **dividing** by 3:

| Ciphertext | ÷ 3 | Resulting ASCII |
|:----------:|:---:|:---------------:|
|   **315**  | 105 |        105      |
|   **324**  | 108 |        108      |
|   **363**  | 121 |        121      |
|   **294**  | 98  |         98      |
|   **294**  | 98  |         98      |
|   **363**  | 121 |        121      |

So we get the ASCII codes:

```
105, 108, 121, 98, 98, 121
```

---

## Converting ASCII to Text

Use any ASCII-to-text converter (e.g., [duplichecker.com ASCII to Text](https://www.duplichecker.com/ascii-to-text.php)) or write a quick script:

```python
codes = [105, 108, 121, 98, 98, 121]
message = ''.join(chr(c) for c in codes)
print(message)  # => ilybby
```

---

## Final Message

```
ilybby
```

Which (with a little creative interpretation) reads as **“I lybby”**  a sweet way of saying **“I love you, baby”**.

---

## Lessons Learned

- **Basic math is powerful**: Sometimes reversing a simple arithmetic operation is all you need.  
- **ASCII fundamentals**: Converting between characters and codes is actually quite common in CTFs.
  
---

## References

- Pecan  
- Python `ord()` and `chr()` documentation  
- [ASCII to Text Converter](https://www.duplichecker.com/ascii-to-text.php)  

---

*Happy hacking :)*
