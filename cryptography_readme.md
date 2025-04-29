# Cryptographic Algorithms Implementation

This repository contains implementations and explanations of various cryptographic algorithms, from classical ciphers to modern encryption standards. Each algorithm is explained in detail with examples, historical context, mathematical foundations, strengths, and weaknesses.

## Table of Contents

- [Caesar Cipher](#caesar-cipher)
- [Rail Fence Cipher](#rail-fence-cipher)
- [Playfair Cipher](#playfair-cipher)
- [RSA](#rsa)
- [DES](#des)

## Caesar Cipher

### Overview
The Caesar Cipher is one of the simplest and oldest known encryption techniques. Named after Julius Caesar, who used it to communicate with his generals, it works by shifting each letter in the plaintext a certain number of positions down the alphabet.

### How It Works
1. Choose a key (shift value) between 1 and 25
2. For each letter in the plaintext:
   - Shift the letter by the key value
   - If shifting goes beyond 'Z', wrap around to the beginning of the alphabet

### Mathematical Representation
For encryption:
```
E(x) = (x + k) mod 26
```

For decryption:
```
D(x) = (x - k) mod 26
```
Where `x` is the position of the plaintext letter in the alphabet (0-25), `k` is the key, and `mod 26` ensures the result stays within the alphabet range.

### Example
- Plaintext: HELLO
- Key: 3
- Ciphertext: KHOOR

Explanation:
- H (7) + 3 = K (10)
- E (4) + 3 = H (7)
- L (11) + 3 = O (14)
- L (11) + 3 = O (14)
- O (14) + 3 = R (17)

### Implementation Considerations
- Case sensitivity: Decide whether to maintain case or convert all to uppercase/lowercase
- Non-alphabetic characters: Decide whether to encrypt these or leave them unchanged
- Alphabet variations: Adapt for different languages or character sets

### Strengths
- Simple to understand and implement
- Fast encryption and decryption
- Requires minimal resources

### Weaknesses
- Extremely vulnerable to brute force attacks (only 25 possible keys)
- Vulnerable to frequency analysis
- Provides minimal security by modern standards

### Historical Significance
The Caesar Cipher represents one of the earliest attempts at encryption and illustrates the fundamental concept of substitution ciphers. While not secure by modern standards, it forms the foundation for understanding more complex cryptographic systems.

## Rail Fence Cipher

### Overview
The Rail Fence Cipher (also called the Zigzag Cipher) is a form of transposition cipher that derives its name from the way in which the plaintext is written out. The letters of the message are written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when the bottom rail is reached. The message is then read off horizontally from each rail.

### How It Works
1. Choose a key (number of rails)
2. Write the plaintext diagonally across the rails
3. Read the ciphertext horizontally across each rail

### Example
- Plaintext: CRYPTOGRAPHY
- Key (rails): 3

Writing out the plaintext in a zigzag pattern:
```
C     O     H
 R   T G   P
  Y P   R A   Y
```

Reading horizontally gives the ciphertext: COHRGTPYPRAY

### Implementation Considerations
- Handling spaces and special characters
- Proper calculation of rail positions
- Edge cases with small messages or large rail counts

### Strengths
- Simple to implement
- Can be performed manually
- Does not require complex mathematical operations

### Weaknesses
- Limited keyspace (number of rails is typically small)
- Vulnerable to known-plaintext attacks
- Patterns can be detected with frequency analysis
- Provides minimal security for short messages

### Historical Significance
The Rail Fence Cipher was used during the American Civil War and is one of the simplest forms of a transposition cipher, where the order of the characters is changed rather than the characters themselves.

## Playfair Cipher

### Overview
The Playfair cipher is a manual symmetric encryption technique that uses a 5x5 grid of letters based on a keyword. It was invented by Charles Wheatstone in 1854 but named after Lord Playfair who promoted its use. It was the first practical digraph substitution cipher, encrypting pairs of letters rather than single letters.

### How It Works
1. Create a 5x5 grid filled with the alphabet, typically combining I and J:
   - Start with the keyword (removing duplicates)
   - Fill the remaining spaces with unused letters in alphabetical order
2. Split the plaintext into pairs of letters (digraphs)
3. Apply the Playfair rules to each pair:
   - If both letters are in the same row, replace with letters to the right (wrapping around)
   - If both letters are in the same column, replace with letters below (wrapping around)
   - If in different rows and columns, replace with letters at the other corners of the rectangle formed

### Example
- Keyword: MONARCHY
- Creates a key square:
```
M O N A R
C H Y B D
E F G I/J K
L P Q S T
U V W X Z
```

- Plaintext: HELLO WORLD
- Prepared plaintext (after splitting and handling duplicate letters): HE LX LO WO RL DX
- Ciphertext: FD MG IK VN QL KV

### Implementation Considerations
- Handling the letter J (typically combined with I)
- Duplicate letters in a pair (often separated with X or another uncommon letter)
- Handling odd-length messages (typically padded with an extra letter)
- Case sensitivity and non-alphabetic characters

### Strengths
- Significantly stronger than simple substitution ciphers
- Resistant to frequency analysis of individual letters
- 600+ possible digraphs vs. 26 monographs in English

### Weaknesses
- Vulnerable to frequency analysis of digraphs
- Small keyspace by modern standards
- Known patterns in language can be exploited

### Historical Significance
The Playfair cipher was used extensively in tactical communications by British forces in World War I and for a time by U.S. and other Allied forces in World War II. It represents an important step in the evolution of cryptography, moving beyond simple substitution while remaining practical for field use.

## RSA (Rivest–Shamir–Adleman)

### Overview
RSA is one of the first practical public-key cryptosystems, widely used for secure data transmission. Named after its inventors (Ron Rivest, Adi Shamir, and Leonard Adleman), it was publicly described in 1977. RSA is based on the practical difficulty of factoring the product of two large prime numbers.

### How It Works
1. **Key Generation**:
   - Select two large prime numbers, p and q
   - Calculate n = p × q
   - Calculate φ(n) = (p-1) × (q-1)
   - Choose an integer e such that 1 < e < φ(n) and gcd(e, φ(n)) = 1
   - Calculate d such that (d × e) mod φ(n) = 1
   - Public key is (n, e)
   - Private key is (n, d)

2. **Encryption**:
   - Ciphertext C = M^e mod n (where M is the plaintext message)

3. **Decryption**:
   - Plaintext M = C^d mod n

### Mathematical Foundation
RSA security is based on the integer factorization problem. While multiplying two large primes is easy, factoring their product is computationally intensive. The security also relies on the difficulty of computing φ(n) without knowing the prime factors of n.

### Example (with small numbers for demonstration)
- Choose p = 61 and q = 53
- Calculate n = 61 × 53 = 3233
- Calculate φ(n) = (61-1) × (53-1) = 60 × 52 = 3120
- Choose e = 17 (relatively prime to 3120)
- Calculate d = 2753 (since (17 × 2753) mod 3120 = 1)
- Public key: (3233, 17)
- Private key: (3233, 2753)

For encrypting the message M = 123:
- C = 123^17 mod 3233 = 855

For decrypting the ciphertext C = 855:
- M = 855^2753 mod 3233 = 123

### Implementation Considerations
- Prime number generation
- Key size (typically 2048 to 4096 bits for modern security)
- Padding schemes (e.g., PKCS#1, OAEP)
- Performance optimizations for large exponentiations

### Strengths
- Asymmetric encryption allows for secure key exchange
- Digital signature capability
- Mathematical foundation based on well-studied problems
- Scalable security (by increasing key size)

### Weaknesses
- Slow compared to symmetric algorithms
- Key generation is computationally intensive
- Vulnerable to quantum computing attacks (Shor's algorithm)
- Implementation vulnerabilities like side-channel attacks

### Applications
- Secure communications (SSL/TLS)
- Digital signatures
- Key exchange protocols
- Secure boot implementations
- Certificate authorities

### Historical Significance
RSA represented a revolution in cryptography by solving the key distribution problem inherent in symmetric algorithms. It enabled secure communications between parties who have never met, forming the foundation of modern e-commerce and secure internet communications.

## DES (Data Encryption Standard)

### Overview
The Data Encryption Standard (DES) is a symmetric-key block cipher published by the U.S. National Bureau of Standards (now NIST) in 1977. It was widely adopted internationally and was the dominant encryption algorithm until the late 1990s, when it became vulnerable to brute force attacks due to its 56-bit key size.

### How It Works
1. **Key Processing**:
   - 64-bit key input (56 bits effective, 8 bits for parity)
   - Key is permuted according to a fixed table (PC-1)
   - Split into two 28-bit halves (C0 and D0)
   - 16 subkeys generated through shifting and permutation

2. **Encryption Process**:
   - Initial permutation (IP) on the 64-bit plaintext block
   - Split into 32-bit halves (L0 and R0)
   - 16 rounds of Feistel network operations:
     - L_i = R_(i-1)
     - R_i = L_(i-1) ⊕ F(R_(i-1), K_i)
     - Where F is the round function:
       - Expansion (E): Expands 32-bit R_(i-1) to 48 bits
       - XOR with round key K_i
       - Substitution (S-boxes): 8 S-boxes convert 48 bits to 32 bits
       - Permutation (P): Rearranges the 32-bit output
   - Final permutation (IP^-1) on the combined block

### S-Box Operation
The S-boxes are the core nonlinear component of DES, transforming 6-bit inputs to 4-bit outputs according to fixed tables. This nonlinearity is crucial for the security of DES.

### Example (simplified)
Due to the complexity of DES, a full example would be extensive. For example, the DES encryption of plaintext "0123456789ABCDEF" (hex) with key "133457799BBCDFF1" (hex) produces the ciphertext "85E813540F0AB405" (hex).

### Implementation Considerations
- Proper key handling (including parity bits)
- Mode of operation (ECB, CBC, CFB, OFB, CTR)
- Initialization vectors for modes other than ECB
- Padding for messages not aligned to 64-bit blocks

### Strengths
- Fast implementation in hardware
- Well-analyzed security properties
- Resistant to differential and linear cryptanalysis (with sufficient rounds)

### Weaknesses
- 56-bit key size (too small by modern standards)
- Vulnerable to brute force attacks
- Some weak keys and semi-weak keys
- ECB mode reveals patterns in data

### Variants and Successors
- **Triple DES (3DES)**: Applies DES three times with different keys
- **Advanced Encryption Standard (AES)**: Official replacement for DES
- **DESX**: DES with additional key whitening steps

### Historical Significance
DES was the first publicly available cryptographic algorithm endorsed by the U.S. government. Its development established important cryptographic principles and processes, including the concept of Feistel networks, S-boxes, and the importance of public cryptanalysis. The shortcomings of DES led to advances in cryptography and ultimately to the development of AES through an open competition.

