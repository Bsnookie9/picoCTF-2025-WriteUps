# Overview
`Easy` `Reverse Engineering` `picoCTF 2025` `browser_webshell_solvable`

# Description
Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it?  
There might be something in it for you.  
The program's source code can be downloaded [here](https://challenge-files.picoctf.net/c_verbal_sleep/9f2b86c1e1068d492f783b106f4535aeb137b0c0e31e43351f8cb82a39456a84/lyric-reader.py).  
Connect to the program with netcat:  
`$ nc verbal-sleep.picoctf.net 56688`

# Hints
- This program can easily get into undefined states. Don't be shy about Ctrl-C.
- Unsanitized user input is always good, right?
- Is there any syntax that is ripe for subversion?

# Solution
Let's first take a look at the source code provided to us:

```python
import re
import time


# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'


song_flag_hunters = secret_intro +\
'''

[REFRAIN]
We’re flag hunters in the ether, lighting up the grid,
No puzzle too dark, no challenge too hid.
With every exploit we trigger, every byte we decrypt,
We’re chasing that victory, and we’ll never quit.
CROWD (Singalong here!);
RETURN

[VERSE1]
Command line wizards, we’re starting it right,
Spawning shells in the terminal, hacking all night.
Scripts and searches, grep through the void,
Every keystroke, we're a cypher's envoy.
Brute force the lock or craft that regex,
Flag on the horizon, what challenge is next?

REFRAIN;

Echoes in memory, packets in trace,
Digging through the remnants to uncover with haste.
Hex and headers, carving out clues,
Resurrect the hidden, it's forensics we choose.
Disk dumps and packet dumps, follow the trail,
Buried deep in the noise, but we will prevail.

REFRAIN;

Binary sorcerers, let’s tear it apart,
Disassemble the code to reveal the dark heart.
From opcode to logic, tracing each line,
Emulate and break it, this key will be mine.
Debugging the maze, and I see through the deceit,
Patch it up right, and watch the lock release.

REFRAIN;

Ciphertext tumbling, breaking the spin,
Feistel or AES, we’re destined to win.
Frequency, padding, primes on the run,
Vigenère, RSA, cracking them for fun.
Shift the letters, matrices fall,
Decrypt that flag and hear the ether call.

REFRAIN;

SQL injection, XSS flow,
Map the backend out, let the database show.
Inspecting each cookie, fiddler in the fight,
Capturing requests, push the payload just right.
HTML's secrets, backdoors unlocked,
In the world wide labyrinth, we’re never lost.

REFRAIN;

Stack's overflowing, breaking the chain,
ROP gadget wizardry, ride it to fame.
Heap spray in silence, memory's plight,
Race the condition, crash it just right.
Shellcode ready, smashing the frame,
Control the instruction, flags call my name.

REFRAIN;

END;
'''

MAX_LINES = 100

def reader(song, startLabel):
  lip = 0
  start = 0
  refrain = 0
  refrain_return = 0
  finished = False

  # Get list of lyric lines
  song_lines = song.splitlines()
  
  # Find startLabel, refrain and refrain return
  for i in range(0, len(song_lines)):
    if song_lines[i] == startLabel:
      start = i + 1
    elif song_lines[i] == '[REFRAIN]':
      refrain = i + 1
    elif song_lines[i] == 'RETURN':
      refrain_return = i

  # Print lyrics
  line_count = 0
  lip = start
  while not finished and line_count < MAX_LINES:
    line_count += 1
    for line in song_lines[lip].split(';'):
      if line == '' and song_lines[lip] != '':
        continue
      if line == 'REFRAIN':
        song_lines[refrain_return] = 'RETURN ' + str(lip + 1)
        lip = refrain
      elif re.match(r"CROWD.*", line):
        crowd = input('Crowd: ')
        song_lines[lip] = 'Crowd: ' + crowd
        lip += 1
      elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
      elif line == 'END':
        finished = True
      else:
        print(line, flush=True)
        time.sleep(0.5)
        lip += 1



reader(song_flag_hunters, '[VERSE1]')
```

Some key points from this source code:
1. The `secret_intro` is the verse that contains our flag
   
   ```python
    secret_intro = \
    '''Pico warriors rising, puzzles laid bare,
    Solving each challenge with precision and flair.
    With unity and skill, flags we deliver,
    The ether's ours to conquer, '''\
    + flag + '\n'
   ```
2. In the source we see that the `scret_intro` is added to the top of the `song_flag_hunter`
   
   ```python
    song_flag_hunters = secret_intro +\
   ```

3. The reader starts at `[VERSE 1]`

   ```python
   reader(song_flag_hunters, '[VERSE1]')
   ```

Using the last key point, we need to find a way to make the reader start from the very **first** line instead of `[VERSE 1]`

When the script takes user input, the input is not sanitized 

```python
elif re.match(r"CROWD.*", line):
        crowd = input('Crowd: ')
        song_lines[lip] = 'Crowd: ' + crowd
        lip += 1
```

> [!IMPORTANT]
> **Unsanitized input** refers to data entered by a user that hasn't been checked or modified to ensure it's safe and doesn't contain malicious code or characters, potentially leading to security vulnerabilities. 

We also know that the RETURN command can be used to force the reader read a specified line:

```python
elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
```

Since we know that the verse containing our flag starts at the very beginning, we need to pass `RETURN 0` to the script.   
However, if we do this, the script will simply echo `RETURN 0` back to us, treating it as a string.   
To make sure the script interprets `RETURN 0` as a command rather than a string, we need to adjust our input slightly.  

Instead of just using `RETURN 0`, we will use `flag;RETURN 0`:


<kbd>![Screenshot](https://github.com/user-attachments/assets/b24fd3f0-5f0d-4abe-9f57-b7f21ca1f7a6)</kbd>

# Flag
`picoCTF{70637h3r_f0r3v3r_75053bc3}`
