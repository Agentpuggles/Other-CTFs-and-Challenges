
# 🕵️‍♂️ PicoCTF Disk Image Analysis: DISKO 1

---

## 1. 🔍 Initial Inspection

### Checking Partitions

```bash
fdisk -l disko-1.dd
```

*Result:* ❌ No partitions listed.

> **Note:** No partition table means the image might be a “super-floppy” — filesystem starts right at byte 0.

---

### Identify Filesystem

```bash
file disko-1.dd
```

*Result:* 📁 Reports a FAT32 volume written directly to the image (no partition table).

---

## 2. 🛠 Mounting the Image

Since it’s a **super-floppy**, mount it directly:

```bash
sudo mkdir /mnt/disko
sudo mount -o ro,loop disko-1.dd /mnt/disko
```

> **Note:** `-o ro,loop` mounts the image read-only with loopback device.

Inside `/mnt/disko/bin` are many executables — but **no obvious “flag” file** 🔎.

---

## 3. 🔎 Searching for the Flag

### Grep in Mounted Filesystem

```bash
grep -R "picoCTF" /mnt/disko
```

*Result:* ❌ No matches found.

> **Insight:** The flag isn’t stored as a normal file or directory entry.

---

### Dumping Strings Directly from the Raw Image

```bash
strings disko-1.dd | grep -i pico
```

*Result:* 🎉 Flag appears immediately:

```
picoCTF{1t5_ju5t_4_5tr1n9_c63b02ef}
```

> **Key Insight:** The flag is hidden in unallocated/slack space — leftover space not assigned to any file.

---

## 4. 📍 Pinpointing the Flag’s Offset

### Find Byte Offset of the Flag

```bash
strings -td disko-1.dd | grep -i pico
```

*Example output:*

```
29779344  picoCTF{…}
```

The flag starts at byte **29,779,344**.

---

### Locate within FAT32 Data Area

* FAT32 data area starts at sector 1608
* Sector size = 512 bytes
* Calculate byte offset:
  $1608 \times 512 = 823,296$ bytes

---

### Calculate Cluster Number Containing the Flag

$$
\text{cluster} = \left\lfloor \frac{29,779,344 - 823,296}{512} \right\rfloor + 2 = 56,556
$$

> **Note:** FAT32 clusters start counting at 2.

---

## 5. 🔨 Carving Out the Cluster

Since cluster **56,556** is unallocated (no directory entry), extract it manually:

```bash
dd if=disko-1.dd of=cluster_56556.bin bs=512 skip=58162 count=1
```

* `skip` = 1608 (data start sector) + (56556 − 2) = 58162 sectors to skip
* `count=1` means extracting 1 cluster (512 bytes)

---

### Verify the Flag in Carved Cluster

```bash
strings cluster_56556.bin | grep -i pico
```

*Result:* 🎯 Flag found again, confirming the slack space location.

---

## 6. 🎉 Conclusion: Flag Location

* Flag was **not inside any visible file or directory**.
* Hidden in **slack/unallocated space** of cluster **56,556** on the FAT32 volume.
* Found only by dumping **all raw strings** and carving out the exact cluster.

---

## 📝 Additional Strings Output Snippet

```
_ZN13QsciScintilla10apiContextEiRiS0_
:/icons/appicon
PICONV      
# $Id: piconv,v 2.8 2016/08/04 03:15:58 dankogai Exp $
piconv -- iconv(1), reinvented in perl
picoCTF{1t5_ju5t_4_5tr1n9_c63b02ef}
runtime.(*piController).reset
...
```

---

Thanks for reading!!!
