# Overview
- Tags: `Easy` `General Skills` `picoCTF 2025` 

# Description
The Rust saga continues? I ask you, can I borrow that, pleeeeeaaaasseeeee?
Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/babfbee79718a6363826ba86300173ffde6d81577e9dd07d4130c53a7eecf6c3/fixme2.tar.gz).

# Hints
* https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html

# Solution
Use the `wget` command to download the rust file

Next use the command `tar -xvzf` followed by the filename to untar the file

Then use vi to edit the `main.src` file and fix the syntax errors:

1. Hit `i` to edit the code
2. Look at the commented lines regarding changeable variables and passing them to a function
> [!IMPORTANT]
> `mut` stands for mutable, allowing a variable to change after inital declaration
3. In the decrypt function, add `&mut` before `String`
4. In the main function, add `mut` in between `let` and `party_foul`
5. In the main function, add `&mut` before `patry_foul`
6. Save the file by hit `ESC` then typing `:wq` + `ENTER`

The main.rs file should now look like this:

```rust
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &mut String){ // How do we pass values to a function that we want to change?

    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}


fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "0d", "c4", "60", "f2", "12", "a0", "18", "03", "51", "03", "36", "05", "0e", "f9", "42", "5b"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let mut party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
    decrypt(encrypted_buffer, &mut party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
}

```

Run the command `cargo run`

```console
┌──(thor㉿kali)-[~/…/2025/General_Skills/fixme2/src]
└─$ cargo run 
   Compiling rust_proj v0.1.0 (/home/thor/CTF/2025/General_Skills/fixme2)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.26s
     Running `/home/thor/CTF/2025/General_Skills/fixme2/target/debug/rust_proj`
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{4r3_y0u_h4v1n5_fun_y31?}

```

# Flag
`picoCTF{4r3_y0u_h4v1n5_fun_y31?}`
