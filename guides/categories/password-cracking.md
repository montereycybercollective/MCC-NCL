# Password Cracking

## Key Tools

| Tool | Primary use | Reference |
|---|---|---|
| CyberChef | Hash analysis and data transformations | [Website](https://gchq.github.io/CyberChef/) |
| hashid | Hash-type identification | [GitHub](https://github.com/psypanda/hashID) |
| hashcat | Password recovery using various attack modes | [Website](https://hashcat.net/hashcat/) |
| John the Ripper | Password recovery using wordlists, masks, and other methods | [Documentation](https://www.openwall.com/john/doc/) |
| Ophcrack | Windows password recovery using rainbow tables | [Project documentation](https://sourceforge.net/p/ophcrack/wiki/ophcrack%20Howto/) |

## Investigate and Prepare

1. **Identify the hash format** - Use multiple tools and compare results, if needed.
  - CyberChef - use the `Analyze hash` operation.
  - hashid - `hashid [hash]`
2. **Prepare the input file** - Place the hashes in a text file (Example: `passwords.txt`).
3. **Select your strategy** - Use the challenge description and any known password patterns to determine which method to try.

> [!TIP]
> Use `username:hash` format in your passwords.txt - makes it easy to read your results!

## Challenge Identification

| Challenge clue                                                | Potential approach                                                                                                          |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| A wordlist is provided or `rockyou.txt` is suggested          | [Wordlist attack](#Wordlist)                                                                 |
| A specific password pattern is given, such as `SKY-TEXT-????` | [Mask attack](#Mask)                                                                                  |
| Passwords contain an unknown word followed by numbers         | [Hybrid mask attack](#Hybrid-Mask)                                                                    |
| Hashes resemble `$y$j9T$sZPOHCdOIBvfkKhVJRSp7` format         | [Identify the specific hash format](#Crypt)                                                           |
| Windows password hashes are involved                          | [Use Ophcrack](#Using-Ophcrack) |

> [!NOTE]
> Challenges can combine these in different configurations. 
> They might also mention something like 'the passwords are all based on [topic]'. This is a hint to google a wordlist of that topic (ie 'password cracking pokemon wordlist').

## Techniques and Procedures

### Wordlist

#### John the Ripper

1. Make sure the `rockyou.txt` wordlist (or another wordlist) is downloaded and available. In Kali Linux, wordlists are commonly located in `/usr/share/wordlists/`.
2. Run John the Ripper with the appropriate wordlist and hash format.

```bash
john --w:/usr/share/wordlists/rockyou.txt --format=Raw-MD5 passwords.txt

# --format= should be the hash you identified. John the Ripper will tell you if you got this wrong.
```

### Mask

#### John the Ripper

```bash
john --mask="SKY-TEXT-?d?d?d?d" --format=[hash type] passwords.txt

# "?d?d?d?d" means there is are four unknown numbers (?d = digit)
```

### Hybrid mask


#### John the Ripper

If a wordlist other than rockyou is offered, use that one in your command instead.

```bash
john -w:[path/to/wordlist] --mask="?w?d?d" --format=[hash type] passwords.txt

# "?w?d?d" means there is and unknown word (?w = word) and two unknown numbers (?d = digit)
```

### Crypt

Hashes containing dollar-sign delimiters may encode information about the hashing.
The hash type is given between the first two '$' delimiters.
Example: Searching `hash type $1$` tells you it is md5.

#### John the Ripper

Example command for `md5crypt` hashes:

```bash
john -w:[path/to/wordlist] --format=md5crypt passwords.txt
```

### View cracked passwords

#### John the Ripper

```bash
john --show passwords.txt
```
### Using Ophcrack

1. Download [Ophcrack](https://ophcrack.sourceforge.io/download.php) (pre-installed in Kali Linux)
2. Download the [XP free fast](https://ophcrack.sourceforge.io/tables.php) wordlist.
3. Follow the instructions outlined [here](https://trove.cyberskyline.com/3f278798f2f6809aa3dad47f2f531509).

## Additional Links

[Password Cracking with Hashcat](https://trove.cyberskyline.com/3097ac9828d94ce1bce2740b0b19895d) - Cyber Skyline