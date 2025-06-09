
# 🧠 Format String 0 — Explainer for Beginners

**Author:** Cheng Zhang
**Challenge Name:** Format String 0
**Category:** Binary Exploitation / CTF
**Difficulty:** Beginner-Friendly
**Audience:** 📚 New to programming, memory, or binary exploitation
**Flag:** 🎉 Yes, we get it at the end!

---

## 📝 What’s Going On?

You’re helping out at a silly burger place called **Pico 'n Patty** 🍔. Two customers—**Patrick** and **Sponge Bob**—want their favorite burgers. You’ll interact with a program by typing in burger names. But there’s a hidden trick behind the scenes 👀.

---

## 💡 Key Idea: Format String Vulnerability

When you use C’s `printf()` function like this:

```c
printf(user_input);
```

Instead of:

```c
printf("%s", user_input);
```

You’re letting the user **control the format**, which can do weird things like:

* 🧨 Print data from memory
* 🧨 Crash the program (called a "segmentation fault")
* 🧨 Trigger secret code (like showing the flag!)

This is called a **format string vulnerability**.

---

## 🔍 Step-by-Step Walkthrough

### 👣 Step 1: Patrick's Turn

You first talk to Patrick, who wants a "giant bite" 🍔.

Program gives you 3 burger options:

```
Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
```

We picked:

```
Gr%114d_Cheese
```

That `%114` is **interpreted by printf as a format**, not as a plain name!

💥 `%114` is trying to print a value from memory – and **that's intentional**. If enough of these are printed (more than 64 characters), it triggers this code:

```c
if (count > 2 * BUFSIZE) {
    serve_bob();
}
```

Which means: "OK, Patrick is happy, now serve Bob!"

✅ You just used **format string abuse** to progress!

---

### 👣 Step 2: Bob’s Turn

Now it gets serious. Bob wants a burger so powerful it could "break the shop" 🤯

Your options are:

```
Pe%to_Portobello, $outhwest_Burger, Cla%sic_Che%s%steak
```

We typed:

```
Cla%sic_Che%s%steak
```

Let’s break that down:

* `%s` = print a string
* `%s%` = confusing and dangerous
* `printf(user_input);` = it runs all of this like a command

💥 This causes a **segmentation fault (crash)**, which the program is actually watching for!

```c
void sigsegv_handler(int sig) {
    printf("\n%s\n", flag);
    ...
}
```

If the program crashes — on purpose — it catches it, and prints the flag 🏴‍☠️!

---

## 🏁 The Final Result

We crashed the program on purpose 💥 and got this output:

```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_74f6c0e7}
```

🎉 **You won!** You used a clever bug to trick the program into giving you the secret.

---

## 📘 TL;DR

| Concept                     | What It Means                                                    |
| --------------------------- | ---------------------------------------------------------------- |
| `printf(user_input)`        | Dangerous if not used carefully                                  |
| `%s`, `%114`, etc.          | Format specifiers used to control what gets printed              |
| Format string vulnerability | When user input is treated as a command instead of data          |
| Segmentation fault          | A crash caused by accessing memory incorrectly                   |
| Exploit                     | Taking advantage of a bug to get something secret (like a flag!) |

---

## 🧠 What You Learned

✅ How C handles strings and `printf`
✅ How bugs can turn into exploits
✅ How to spot and use format string vulnerabilities
✅ How segmentation faults can reveal secrets (with a handler)

---

## 🧰 Tools Used

* `nc` (netcat) to connect to the challenge
* `file` to inspect the binary
* Reading C code
* Curiosity 😄

---

## 🎓 Want to Learn More?

* [Format String Vulnerabilities - OWASP](https://owasp.org/www-community/attacks/Format_string_attack)
* Try more challenges at [https://picoctf.org](https://picoctf.org)

---

Hope you had fun learning! 🧑‍🏫✨

---
