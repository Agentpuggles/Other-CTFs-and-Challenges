# ABC Company Challenge Write-Up

Heya guys! Today I’m doing the **ABC Company** challenge on **Pecan**!!!

---

## Challenge Info

- **Name**: ABC Company  
- **Difficulty**: Medium  
- **Description**:  
  > You infiltrated a company and captured some network traffic... can you find the flag?  
  > **Hint**: You might need Wireshark (or use [A-Packets.com](https://a-packets.com))  
- **File**: Provided as a `.pcap` (must unzip first)  
- **Author**: Huu Nguyen  

---

## Tools Used

I’m using **Wireshark** on **Kali Linux**, but this works just as well on Windows or other Linux distros too.

---

## Getting Started

1. Open Wireshark.
2. Go to **File → Open**, and select the `.pcap` file you unzipped.
3. Click **Open** to load the captured network traffic.

You'll see a list of packets (called *frames* in Wireshark). There are **878 frames** in this capture, so manually scrolling through them isn't efficient.

---

## Using Wireshark Filters

Instead of digging through every packet, we’ll use Wireshark’s built-in filter feature:

### Search for:  
```wireshark
frame contains "pecan"
```

This tells Wireshark to look for any packets that contain the word `pecan`, which is a great guess since all Pecan CTF flags use that prefix.

Boom! We get this match in **Frame 394**:

```
394	128.523045963	192.168.175.132	192.168.175.128	TELNET	121	55 bytes data
```

### Frame 394 Details:
```
Data: msfadmin@metasploitable:~$ echo pecan{w0w_you_g0t_it}
```

---

## 🎉 Flag

```
pecan{w0w_you_g0t_it}
```

---

## Final Thoughts

Short and sweet challenge. We got to play with Wireshark and see how useful filtering can be when handling large packet captures.

Thanks for reading this short one, y’all!
