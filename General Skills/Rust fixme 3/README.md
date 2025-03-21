# Overview
- Tags: `Easy` `General Skills` `picoCTF 2025` 

# Description
Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!  
Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/dcdaf491b35c1d0f5075e9583edbbb7aaea1dffb6ad32bc000e4d87b5200ff7b/fixme3.tar.gz).

# Hints
* Read the comments...darn it!

# Solution
Use the `wget` command to download the rust file

Next use the command `tar -xvzf` followed by the filename to untar the file

Then use vi to edit the `main.src` file and fix the syntax errors:

1. Hit `i` to edit the code
2. Remove the `//` from the `unsafe` method, which is commenting the function and not allowing the file to run correctly
3. Save the file by hit `ESC` then typing `:wq` + `ENTER`

Run the command `cargo run`

```console
┌──(thor㉿kali)-[~/…/2025/General_Skills/fixme3/src]
└─$ cargo run  
   Compiling rust_proj v0.1.0 (/home/thor/CTF/2025/General_Skills/fixme3)
warning: use of deprecated method `xor_cryptor::XORCryptor::<xor_cryptor::V1>::decrypt_vec`
  --> src/main.rs:24:36
   |
24 |         let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
   |                                    ^^^^^^^^^^^
   |
   = note: `#[warn(deprecated)]` on by default

warning: `rust_proj` (bin "rust_proj") generated 1 warning
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.30s
     Running `/home/thor/CTF/2025/General_Skills/fixme3/target/debug/rust_proj`
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{n0w_y0uv3_f1x3d_1h3m_411}

```

# Flag
`picoCTF{n0w_y0uv3_f1x3d_1h3m_411}`
