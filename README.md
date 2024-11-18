# Image Steganography Tool

This is a simple **Encryption** and **Steganography** tool written in C++. It uses password-protected encryption to secure a file's contents and embeds it into an image's pixel data using Least-Significant-Bit (LSB) encoding.

### Why I Made It  
I created this tool as a fun, educational project while exploring the concepts of steganography and cryptography. As a computer science student, I wanted to learn more about securing data and embedding it in creative ways. This tool is not intended for professional use but rather as a proof-of-concept to demonstrate how steganography works.

### How It Works  
The tool operates in two modes:  
1. **Encoding**: Embeds a file into an image by encrypting the file and hiding it in the least significant bits of the image's pixel data.  
2. **Decoding**: Extracts and decrypts the embedded file from an image, validating its integrity using a checksum.  

The encryption process uses advanced techniques like AES-256-CBC and PBKDF2-HMAC-SHA-256 for strong security, while the steganography process ensures the data is seamlessly hidden in the image.

---

## Encoding

```bash
./sksteg encode -i input_image.png -e example.zip -o output.png
Password: 1234
* Image size: 640x426 pixels
* Encoding level: Low (Default)
* Max embed size: 132.38 KiB
* Embed size: 61.77 KiB
* Encrypted embed size: 61.78 KiB
* Generated CRC32 checksum
* Generated encryption key with PBKDF2-HMAC-SHA-256 (20000 rounds)
* Encrypted embed with AES-256-CBC
* Embedded example.zip into image
```

---

## Usage

```bash
Usage: sksteg [-h] {decode,encode}

Optional arguments:
  -h, --help   	shows help message and exits

Subcommands:
  decode        Decodes and extracts an embedded file from an image
  encode        Encodes a file into an image
```

---

## Theory of Operation

### Encoding
1. A *128-bit Password Salt* and a *128-bit AES Initialization Vector* are generated using binary data from **/dev/urandom**.  
2. The user’s password is used with **PBKDF2-HMAC-SHA-256** to derive an encryption key.  
3. A **CRC32** checksum of the file is calculated and stored as a header for validation.  
4. The file is padded using **PKCS #7** and encrypted with **AES-256-CBC**.  
5. The encrypted data is embedded into the image by modifying the least significant bits of each pixel's channel bytes, starting at a random offset.

### Decoding
1. The program extracts and decrypts the data from the image.  
2. It verifies the file signature and **CRC32** checksum from the header to ensure integrity.  
3. If the extracted data fails verification (e.g., due to a wrong password or corrupted image), the decryption process is aborted.

### Detection
While identifying the presence of embedded data is possible, the encryption ensures the hidden data cannot be decrypted without the correct password. This method protects against unauthorized access, but the program should not be considered foolproof.

---

## Disclaimer

This program is a simple proof-of-concept created for educational purposes and personal learning. **Do not rely on this tool to encrypt and hide sensitive or critical data.** I am not a cryptography expert; use this program at your own risk.