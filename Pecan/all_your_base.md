# 🔓 All Your Base — Challenge Writeup

**Category:** Reverse Engineering  
**Difficulty:** Advanced  
**Platform:** Linux  
**Challenge Name:** All Your Base  
**Flag:** `pecan{411Y0UR8453}`

---

## 🧩 Challenge Overview

We were provided with a C source file `All_your_base.c` and no other dependencies.

```bash
└─$ ls
All_your_base.c
````

Upon inspection, the program includes a humorous reference to the classic meme *“All your base are belong to us”*, and implements a dramatic countdown with a prompt for an access code.

However, there’s an obstacle! the code calls a non-standard function:

```c
char* code = base64_decode("NDExWTBVUjg0NTM=");
```

And also includes this at the top:

```c
#include "base64.h"
```

But no `base64.h` or `base64.c` is provided.

---

## 🔍 Static Analysis

We can deduce that the provided string is base64-encoded:

```c
// base64: "NDExWTBVUjg0NTM="
// Decodes to: "411Y0UR8453"
```

This hints that the **actual access code** the program expects is:

```
411Y0UR8453
```

We can also infer the logic flow:

* User input is compared with the decoded base64 value.
* If matched, a "Correct" message is displayed with the code, and instructions to submit its **MD5 hash**.

---

## 🛠️ Fixing the Compilation

Since the program depends on a missing `base64_decode()` function, we **patched the code** to directly use the decoded string.

### 🔧 Edited `All_your_base.c`:

```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>

int main() {
   setbuf(stdout, 0);

   // Meme-style intro sequence
   printf("Main screen turn on\n");
   sleep(3);
   printf("How are you\n");
   sleep(2);
   printf("All your base are belong to us\n");
   sleep(2);
   printf("Guess the access code or you have no survive to make your time\n");
   sleep(2);
   printf("HA HA HA HA HA\n");
   sleep(3);

   // Access code input
   char guess[13];
   printf("Enter the access code (Hint: You are the l33t!!):  ");
   scanf("%13s", guess);

   char buffer[13];
   strncpy(buffer, guess, 13);
   buffer[12] = '\0';

   // Replaced missing base64 decode with hardcoded value
   char* code = "411Y0UR8453";

   if (strcmp(buffer, code) == 0) {
      printf("Correct, your flag is: %s, Please submit flag in it's MD5 format\n", code);
   } else {
      printf("Incorrect\n");
   }

   return 0;
}
```

---

## 🚀 Running the Program

### ✅ Compile:

```bash
gcc All_your_base.c -o allyourbase
```

### ✅ Run:

```bash
./allyourbase
```

After a series of messages, it will prompt:

```
Enter the access code (Hint: You are the l33t!!):
```

Input the correct code:

```
411Y0UR8453
```

You’ll get:

```
Correct, your flag is: 411Y0UR8453, Please submit flag in it's MD5 format
```

---

## 🔐 Getting the Flag

Although the program asks for an **MD5 hash**, the actual flag format for this challenge was:

```
pecan{411Y0UR8453}
```

## 💡 Key Takeaways

* Always examine the source for **hardcoded secrets** or **incomplete dependencies**.
* If a function like `base64_decode()` is missing, you can often reverse-engineer what it was doing.

---

Thanks for reading!!
