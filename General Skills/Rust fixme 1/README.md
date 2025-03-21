# Overview
- Tags: `Easy` `General Skills` `picoCTF 2025` 

# Description
Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!  
Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/3f0e13f541928f420d9c8c96b06d4dbf7b2fa18b15adbd457108e8c80a1f5883/fixme1.tar.gz).

# Hints
* Cargo is Rust's package manager and will make your life easier. See the getting started page [here](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
* [println!](https://doc.rust-lang.org/std/macro.println.html)
* Rust has some pretty great compiler error messages. Read them maybe?

# Solution
Use the `wget` command to download the rust file

Next use the command `tar -xvzf` followed by the filename to untar the file

After untarring the file, a new set of directories and files are created:

```
┌──(thor㉿kali)-[~/CTF/2025/General_Skills]
└─$ tar -xvzf fixme1.tar.gz
fixme1/
fixme1/Cargo.toml
fixme1/Cargo.lock
fixme1/src
fixme1/src/main.rs
```

We need to change directories to the `/src` directory to access the `main.rs` file

Now let's look for syntax errors. There are a few comments that point out those errors

Let's use the vi editor to edit the `main.src` file and fix the syntax errors:

1. Hit `i` to edit the code
2. The first syntax error is the missing semicolon after `("CSUCKS")`. Let's put a semicolon there
3. The second syntax error is the missing return statement after the decryption key if statement. Let's type `return`
4. The third syntax error is the incorrect use of the println function. Let's change `":?"` to `{}`
5. Save the file by hit `ESC` then typing `:wq` + `ENTER`

As the first hint mentions, Cargo is Rust's package manager. Let's use the command `cargo run`

```console
┌──(thor㉿kali)-[~/…/2025/General_Skills/fixme1/src]
└─$ cargo run  
   Compiling rust_proj v0.1.0 (/home/thor/CTF/2025/General_Skills/fixme1)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.68s
     Running `/home/thor/CTF/2025/General_Skills/fixme1/target/debug/rust_proj`
picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}
```

# Flag
`picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}`
