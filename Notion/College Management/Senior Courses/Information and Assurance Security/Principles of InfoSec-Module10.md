Hello. We have spent the last few modules discussing the architectural, legal, and personnel-based layers of security. Now, we arrive at the mathematical heart of information security: **Cryptography**.

As an engineer, I view cryptography as the ultimate enforcement mechanism. Policies tell people not to read sensitive data; firewalls try to block access to it; but **cryptography** ensures that even if they steal the data, it remains useless noise.

This lecture covers **Module 10**. We will trace the evolution of codes from ancient history to modern quantum-resistant algorithms, dissect the mechanics of symmetric and asymmetric systems, and explore the protocols that secure the Internet today.

---

# Lecture: Cryptography — The Science of Secret Writing

## I. Foundations and History

We begin with definitions. The field is **Cryptology**, which encompasses two opposing disciplines:

1. **Cryptography:** The art of making codes (securing information).
2. **Cryptanalysis:** The art of breaking codes (obtaining plaintext without the key).

**Historical Context** Cryptography is not new.

- **1900 BC:** Egyptian hieroglyphs used symbol transformation to obscure meaning.
- **400 BC:** The Spartans used the **Scytale**, a tool for transposition (scrambling) where a strip of parchment was wrapped around a baton of specific diameter.
- **50 BC:** Julius Caesar used the **Caesar Cipher**, shifting the alphabet by three letters (Substitution).
- **WWII:** The Germans used the **Enigma machine**, a complex polyalphabetic substitution engine. Its breaking by the Allies (Ultra) was pivotal to the war effort.

**Core Concepts** To understand the math, you must know the variables:

- **Plaintext (M):** The original message.
- **Ciphertext (C):** The encrypted, unreadable message.
- **Cipher/Algorithm:** The mathematical formula used for transformation.
- **Key (Cryptovariable):** The secret value required by the algorithm to encrypt/decrypt.
- **Keyspace:** The total number of possible keys (e.g., a 3-bit key has a keyspace of 8),.

---

## II. Cipher Methods: The Mechanics of Encryption

Algorithms generally function via two methods: **Bit Stream** (converting one bit at a time) or **Block Cipher** (converting chunks of data).

### 1. Substitution Ciphers

This method replaces one value with another.

- **Monoalphabetic:** Uses a single alphabet. (e.g., A always equals D). These are easily broken via frequency analysis (e.g., 'E' is the most common letter in English).
- **Polyalphabetic:** Uses multiple alphabets. The **Vigenère cipher** is the classic example. It uses a keyword to shift between different substitution alphabets, making frequency analysis much harder.

### 2. Transposition Ciphers

This method rearranges the values (permutation). The data remains the same; the _order_ changes.

- _Example:_ The "Rail Fence" cipher writes the message across two lines and reads it zig-zag.

### 3. The Exclusive OR (XOR)

This is a fundamental Boolean logic operation used in modern digital cryptography.

- **The Logic:** If two bits are the same, the result is 0. If they are different, the result is 1.
    - 0 XOR 0 = 0
    - 1 XOR 0 = 1
    - 1 XOR 1 = 0
- **Reversibility:** Ideally suited for crypto because (A XOR Key) = Cipher, and (Cipher XOR Key) = A. It is simple but weak if used alone.

### 4. The Vernam Cipher (One-Time Pad)

Developed at AT&T, this is the "perfect" cipher. It uses a set of characters (the pad) only once.

- **The Rule:** The key must be as long as the message, completely random, and used only once.
- **The Math:** It typically uses modulo arithmetic (subtracting 26 if the sum of the text and key values exceeds 26). If followed perfectly, it is unbreakable.

### 5. Hash Functions

Hashing is not encryption; it is a one-way mathematical fingerprint. You cannot "decrypt" a hash.

- **Purpose:** To verify integrity. If you change one bit of a file, the hash value changes completely.
- **Algorithms:** SHA-1 (160-bit, legacy) and SHA-2 (secure).
- **Rainbow Tables:** Attackers use tables of pre-computed hashes to crack passwords. To defeat this, we use **Salting**—adding random data to the password before hashing so the pre-computed tables fail.

---

## III. Modern Algorithms: Symmetric vs. Asymmetric

This is the most critical architectural distinction in cryptography.

### 1. Symmetric Encryption (Private Key)

The same key is used to encrypt and decrypt.

- **Pros:** Extremely fast and efficient.
- **Cons:** **Key Distribution.** How do I send you the secret key without an attacker intercepting it? It requires "out-of-band" exchange (e.g., a face-to-face meeting).
- **Major Algorithms:**
    - **DES (Data Encryption Standard):** 56-bit key. Obsolete and crackable.
    - **3DES (Triple DES):** Runs DES three times. Secure but slow.
    - **AES (Advanced Encryption Standard):** The current gold standard (FIPS 197). Based on the Rijndael algorithm. It uses block sizes of 128 bits and key sizes of 128, 192, or 256 bits.

### 2. Asymmetric Encryption (Public Key)

Uses two mathematically related keys: a **Public Key** (visible to everyone) and a **Private Key** (kept secret).

- **The Mechanism:** If I encrypt with your Public Key, only your Private Key can decrypt it.
- **Pros:** Solves the key distribution problem. I can send you a message securely without ever meeting you.
- **Cons:** Computationally intense and slow.
- **Major Algorithm:** **RSA** (Rivest-Shamir-Adleman). Security relies on the difficulty of factoring large prime numbers.

### 3. Hybrid Systems (Best of Both Worlds)

In practice, we use Asymmetric encryption to exchange a Symmetric key.

- **Diffie-Hellman Key Exchange:** A method to securely exchange session keys over an insecure medium. Once the session key (symmetric) is exchanged, communication switches to symmetric encryption for speed.

---

## IV. Cryptographic Tools and Infrastructure

### 1. Public Key Infrastructure (PKI)

PKI is the system of hardware, software, and policies that manages digital certificates and public-key encryption.

- **Certificate Authority (CA):** The trusted entity (like VeriSign or a corporate server) that issues and manages certificates.
- **Registration Authority (RA):** Verifies the user's identity before a certificate is issued.
- **Digital Certificate:** A digital file (X.509 format) containing the user's public key and identity, signed by the CA,.

### 2. Digital Signatures (Non-Repudiation)

How do we prove _who_ sent a message?

- **The Process:** The sender hashes the message and encrypts the hash with their **Private Key**.
- **Verification:** The receiver decrypts the signature with the sender's **Public Key**. If it works, it proves the sender signed it (Non-Repudiation) and the message hasn't changed (Integrity).

### 3. Steganography

The art of "hiding" messages. Unlike cryptography (which scrambles data), steganography hides the existence of the data—for example, hiding text bits inside the color codes of a digital image file.

---

## V. Protocols for Secure Communication

We apply these algorithms via protocols to secure the Internet.

### 1. Secure Web (HTTP vs. HTTPS)

- **SSL (Secure Sockets Layer):** Developed by Netscape. Uses public key encryption to secure the channel. Now largely replaced by TLS.
- **TLS (Transport Layer Security):** The successor to SSL.
- **HTTPS:** HTTP over SSL/TLS. It provides encryption, identification, and integrity for web traffic.

### 2. Secure Email

- **S/MIME:** Uses digital signatures and public keys to secure email.
- **PGP (Pretty Good Privacy):** A hybrid system developed by Phil Zimmermann. It uses the "Web of Trust" model rather than a centralized CA. It uses IDEA for data encryption and RSA for key exchange,.

### 3. Secure Networks (IPSec)

**IPSec** creates a secure framework for IP networks (VPNs). It has two modes:

1. **Transport Mode:** Encrypts only the data payload. The header remains visible.
2. **Tunnel Mode:** Encrypts the entire packet (header + payload) and wraps it in a new header. This is more secure as it hides the destination,.

### 4. Wireless Security

- **WEP (Wired Equivalent Privacy):** Early standard. Flawed and easily broken.
- **WPA/WPA2 (Wi-Fi Protected Access):** Uses AES and dynamic keys. WPA2 is the current standard for robust security.
- **Bluetooth:** Short-range wireless. Vulnerable to interception if not properly paired and secured,.

---

## VI. Attacks on Cryptography

As engineers, we must know how our systems break.

- **Brute Force:** Trying every possible key combination.
    - _Defense:_ Use longer keys. A 256-bit key is exponentially stronger than a 56-bit key.
- **Dictionary Attack:** Trying common words or passwords.
- **Rainbow Tables:** Using pre-computed hash databases to reverse passwords.
- **Man-in-the-Middle:** Intercepting a public key exchange and substituting a fake key to eavesdrop.
- **Timing Attacks:** Measuring how long the CPU takes to perform encryption to deduce the key.

---

## VII. Summary

Cryptography is the transformation of readable data into unreadable code to ensure:

1. **Confidentiality** (via Encryption).
2. **Integrity** (via Hashing).
3. **Authentication** and **Non-Repudiation** (via Digital Signatures and PKI).

While algorithms like AES and RSA are mathematically sound, the weakness usually lies in **Key Management** (losing the key or letting it be stolen) or **Implementation** (using outdated protocols like WEP or DES). Security is not just about the math; it is about the rigorous application of that math in a hostile environment.