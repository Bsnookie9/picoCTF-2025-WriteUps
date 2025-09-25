# Overview
- Tags: `Easy` `Cryptography` `picoCTF 2025` `browser_webshell_solvable` 

# Description
A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords.   
Can you gain access to the secret stored within the server?  

Connect to the program with netcat:  
`$ nc verbal-sleep.picoctf.net _____`

# Hints
* Understanding hashes is very crucial. [Read more here](https://primer.picoctf.org/#_hashing)
* Can you identify the hash algorithm? Look carefully at the length and structure of each hash identified.
* Tried using any hash cracking tools?

# Solution
Simply enter the command provided then hit `Enter`

You are then prompted with this:
```
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash:
```

From here we can see that the hash provided is smaller in length (32 characters), meaning it is an MD5 hash.

We can use a password cracker like `hashcat` to crack the hash. Before we can use hashcat, we need to get the hash into a file. 
Using the `echo` command, we can copy the hash and create a file:
`echo "<hash>" > hash_#`

Using the hashcat command, there are parameters to follow. Here is the syntaxt for using hashcat
```
hashcat -a <attack_mode> -m <hash_type> <hash_file> <wordlist>
```
* We will use the dictionary attack mode   
* Use [this](https://hashcat.net/wiki/doku.php?id=example_hashes) to find the value you need based on the hash type (MD5, SHA-1, etc.)
* The file naming convention I am using is hash_#
* The wordlist we will use is in /usr/share/wordlists/rockyou.txt

The first hashcat command should look like this:
```js
┌──(thor㉿kali)-[~/CTF/2025/Cryptography/hashcrack]
└─$ hashcat -a 0 -m 0 hash_1 /usr/share/wordlists/rockyou.txt
```

First password is `password123`

Enter the password and you are then prompted with another hash:

```
Correct! You've cracked the MD5 hash with no secret found!

Flag is yet to be revealed!! Crack this hash: b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```
This hash is 40 characters long which means that it is a SHA-1 hash. 

The second hashcat command should look like this:
```js
┌──(thor㉿kali)-[~/CTF/2025/Cryptography/hashcrack]
└─$ hashcat -a 0 -m 100 hash_2 /usr/share/wordlists/rockyou.txt
```

Second password is `letmein`

Enter the password and you are then prompted with a final hash:

```
Correct! You've cracked the SHA-1 hash with no secret found!

Almost there!! Crack this hash: 916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```
This hash is the longest of the three and looks like a SHA-256 hash (64 characters long). 

The third hashcat command should look like this:
```js
┌──(thor㉿kali)-[~/CTF/2025/Cryptography/hashcrack]
└─$ hashcat -a 0 -m 1400 hash_3 /usr/share/wordlists/rockyou.txt
```

Third, and final, password is `qwerty098`

```
Correct! You've cracked the SHA-256 hash with a secret found. 
The flag is: picoCTF{UseStr0nG_h@shEs_&PaSswDs!_93e052d7}
```

# Flag
`picoCTF{UseStr0nG_h@shEs_&PaSswDs!_93e052d7}`
