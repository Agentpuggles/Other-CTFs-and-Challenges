# Encoded Challenge Write-Up

Hey my fellow cybersecurity enthusiasts! Welcome back to my little documenting thing it’s like when the teacher asks you to show your work hehe.

Today we are doing the Pecan challenge called **Encoded**.

---

## Table of Contents

1. [Challenge Description](#challenge-description)  
2. [Setup & File Inspection](#setup--file-inspection)  
3. [Opening in a Hex/Text Editor](#opening-in-a-hextext-editor)  
4. [Using `strings`](#using-strings)  
5. [Identifying the Base64 String](#identifying-the-base64-string)  
6. [Decoding with CyberChef](#decoding-with-cyberchef)  
7. [Final Flag](#final-flag)  
8. [Lessons Learned](#lessons-learned)  

---

## Challenge Description

- **Name**: Encoded  
- **Difficulty**: Easy  
- **Description**:  
  > The flag is encoded, but not encrypted, in the ELF binary.  
  > Find the encoded data to get the flag.

- **Download**: Provided ELF file named `encoded`  
- **Hint**: You might find a Linux terminal window useful…

---

## Setup & File Inspection

I’m using Kali Linux for this:

```bash
$ ls
encoded

$ file encoded
encoded: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked,
         for GNU/Linux 3.2.0, not stripped
````

We can see it’s a 64-bit ELF binary. Let’s take a look!

---

## Opening in a Hex/Text Editor

I opened the file in Sublime, but all I saw was raw bytes:

```
7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
02 00 3e 00 01 00 00 00 60 10 40 00 00 00 00 00
...
```

There’s nothing human-readable here, so let’s try another approach.

---

## Using `strings`

The `strings` utility pulls out printable ASCII:

```bash
$ strings encoded
/lib64/ld-linux-x86-64.so.2
malloc
__libc_start_main
free
printf
...
notaflag
nothing
cGVjYW57QkBzaWNhbGx5X0lfU2gwdWxkX0gxZGVfQjN0dDNyfQ==
GCC: (Debian 11.3.0-5) 11.3.0
...
```

Most of these are library functions or section names, but one line jumps out:

```
cGVjYW57QkBzaWNhbGx5X0lfU2gwdWxkX0gxZGVfQjN0dDNyfQ==
```

---

## Identifying the Base64 String

Whenever you see a long string of letters, numbers, “+”, “/”, and ending in “==”, it’s almost certainly Base64. Let’s decode it!

---

## Decoding with CyberChef

1. Open [CyberChef](https://gchq.github.io/CyberChef/).

2. In the **Operations** panel, search for **From Base64** and drag it in the **Recipe** panel.

3. Paste our string into the **Input** panel:

   ```
   cGVjYW57QkBzaWNhbGx5X0lfU2gwdWxkX0gxZGVfQjN0dDNyfQ==
   ```

4. The **Output** panel shows:

   ```
   pecan{B@sically_I_Sh0uld_H1de_B3tt3r}
   ```

---

## Final Flag

```
pecan{B@sically_I_Sh0uld_H1de_B3tt3r}
```

Congratulations! 🎉 We found the flag using simple inspection and a Base64 decode.

---

## Lessons Learned

* **`strings` is your friend** when you need to pull readable data from binaries.
* Recognizing Base64 patterns (especially “==” padding) is crucial.
* CyberChef makes quick work of common encodings without writing custom scripts.

Thanks for reading, y’all! 😊
