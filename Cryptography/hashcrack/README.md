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

From here we can see that the hash provided is smaller in length, meaning it is most likely an MD5 hash.

We can use a password cracker like `hashcat` to crack the hash (SCREENSHOT NEEDED)

First password is `password123`

Enter the password and you are then prompted with another hash:

```
Correct! You've cracked the MD5 hash with no secret found!

Flag is yet to be revealed!! Crack this hash: b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```
This hash is a little longer than the previous one, but still short and most likely is a SHA-1 hash. (SCREENSHOT NEEDED)

Second password is `letmein`

Enter the password and you are then prompted with a final hash:

```
Correct! You've cracked the SHA-1 hash with no secret found!

Almost there!! Crack this hash: 916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```
This hash is the longest of the three and looks like a SHA-256 hash. (SCREENSHOT NEEDED)

Third, and final, password is `qwerty098`

```
Correct! You've cracked the SHA-256 hash with a secret found. 
The flag is: picoCTF{UseStr0nG_h@shEs_&PaSswDs!_93e052d7}
```

# Flag
`picoCTF{UseStr0nG_h@shEs_&PaSswDs!_93e052d7}`
