# Computational Theory Year 4 – SHA-256 

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![NumPy](https://img.shields.io/badge/numpy-%3E%3D1.21.0-orange.svg)](https://numpy.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A complete dive into the **SHA-256 cryptographic hash function** in Python, built for educational purposes as part of the Computational Theory module (Year 4).

This project demonstrates an understanding of cryptographic primitives by implementing components of SHA-256 according to the official [NIST FIPS 180-4 Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf).

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Implementation Details](#-implementation-details)
- [Testing & Verification](#-testing--verification)
- [Dependencies](#-dependencies)
- [Background & References](#-background--references)
- [Security Considerations](#-security-considerations)
- [Future Improvements](#-future-improvements)
- [Acknowledgments](#-acknowledgments)

---

## 🎯 Overview

This repository contains a comprehensive implementation of the **SHA-256** (Secure Hash Algorithm 256-bit) cryptographic hash function, developed using Python and NumPy.

### What is SHA-256?

SHA-256 is a cryptographic hash function that:
- Takes an input of any length
- Produces a fixed 256-bit (32-byte) output
- Is deterministic (same input always produces same output)
- Is computationally infeasible to reverse
- Exhibits the avalanche effect (small input changes drastically alter output)

SHA-256 is widely used in:
- **Blockchain & Cryptocurrencies** (Bitcoin, Ethereum)
- **Digital Signatures & Certificates** (SSL/TLS)
- **Password Storage** (when combined with proper salting)
- **Data Integrity Verification** (file checksums)
- **Version Control Systems** (Git commit hashes)

---

## ✨ Features

- ✅ **Complete SHA-256 Implementation** - Bitwise operations, constants, and compression functions
- ✅ **Constant Generation** - Derives SHA-256 constants from prime number roots (no hardcoding)
- ✅ **Message Padding** - Implements official NIST padding scheme for arbitrary-length inputs
- ✅ **Block Processing** - Generator-based 512-bit block parser
- ✅ **Fully Tested** - Verified against Python's `hashlib` and official test vectors
- ✅ **Educational Focus** - Extensive documentation and step-by-step explanations
- ✅ **Dictionary Attack Demo** - Demonstrates password cracking vulnerabilities

---

## 📁 Project Structure
```
computational-theory/
│
├── problems.ipynb           # Main Jupyter notebook with all implementations
├── requirements.txt         # Python dependencies
├── README.md               # This file
├── .gitignore              # Git ignore patterns
```

### Notebook Structure (problems.ipynb)

The notebook is organized into 5 progressive problems:

1. **Problem 1: Binary Words and Operations**
   - Bitwise functions: `Ch`, `Maj`, `Parity`
   - Sigma functions: `Σ0`, `Σ1`, `σ0`, `σ1`
   - Rotation and shift operations

2. **Problem 2: Fractional Parts of Cube Roots**
   - Generates 64 round constants from prime cube roots
   - Verification against official SHA-256 constants

3. **Problem 3: Message Padding**
   - NIST-compliant 512-bit block padding
   - Generator function for efficient block processing

4. **Problem 4: Hash Computation**
   - Complete SHA-256 hash function
   - Message scheduling and compression
   - Integration of all previous components

5. **Problem 5: Passwords and Dictionary Attacks**
   - Demonstrates hash function security implications
   - Dictionary attack implementation
   - Password cracking demonstration

---

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- pip (Python package installer)
- Jupyter Notebook or JupyterLab (optional, for interactive use)

### Setup

1. **Clone the repository**
```bash
   git clone https://github.com/JakubKopras6/computational-theory-yr4
   cd computational-theory
```

2. **Create a virtual environment** (recommended)
```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On macOS/Linux
   source venv/bin/activate
```

3. **Install dependencies**
```bash
   pip install -r requirements.txt
```

4. **Launch Jupyter Notebook**
```bash
   jupyter notebook problems.ipynb
```
   
   Or use VS Code, JupyterLab, or Google Colab.

---

### Running the Notebook

1. Open `problems.ipynb` in Jupyter
2. Run cells sequentially from top to bottom
3. Each problem builds on the previous one
4. Test outputs are included for verification

### Comparing with Python's hashlib
```python
import hashlib

# Our implementation
our_hash = sha256(b"test")

# Python's built-in
expected_hash = hashlib.sha256(b"test").hexdigest()

print(f"Match: {our_hash == expected_hash}")
# Output: Match: True
```

---

## 🔬 Implementation Details

### Problem 1: Bitwise Operations

Implements the core logical functions defined in [FIPS 180-4 Section 4.1](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf#page=10):

- **Ch(x, y, z)** - Choose function: `(x ∧ y) ⊕ (¬x ∧ z)`
- **Maj(x, y, z)** - Majority function: `(x ∧ y) ⊕ (x ∧ z) ⊕ (y ∧ z)`
- **Σ₀(x)** - `ROTR²(x) ⊕ ROTR¹³(x) ⊕ ROTR²²(x)`
- **Σ₁(x)** - `ROTR⁶(x) ⊕ ROTR¹¹(x) ⊕ ROTR²⁵(x)`
- **σ₀(x)** - `ROTR⁷(x) ⊕ ROTR¹⁸(x) ⊕ SHR³(x)`
- **σ₁(x)** - `ROTR¹⁷(x) ⊕ ROTR¹⁹(x) ⊕ SHR¹⁰(x)`

All operations use NumPy's `uint32` type to ensure proper 32-bit arithmetic.

### Problem 2: Round Constants

Generates the 64 round constants K₀...K₆₃ by:
1. Finding the first 64 prime numbers
2. Computing their cube roots
3. Extracting fractional parts
4. Scaling to 32-bit integers: `K = ⌊frac(∛prime) × 2³²⌋`

These constants match the official values in [FIPS 180-4 Section 4.2.2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf#page=11).

### Problem 3: Message Padding

Implements padding per [FIPS 180-4 Section 5.1.1](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf#page=13):

1. Append bit '1' (byte `0x80`)
2. Append '0' bits until `length ≡ 448 (mod 512)`
3. Append 64-bit message length in bits (big-endian)
4. Yield 512-bit (64-byte) blocks

### Problem 4: Hash Computation

Implements the main compression function per [FIPS 180-4 Section 6.2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf#page=22):

1. **Message Schedule**: Expands 16 words to 64 words using σ functions
2. **Initialize**: Sets working variables a-h from current hash
3. **64 Rounds**: Applies compression function with constants and message schedule
4. **Addition**: Adds compressed values back to hash state

Initial hash values H₀...H₇ are derived from square roots of the first 8 primes.

### Problem 5: Dictionary Attack

Demonstrates why raw SHA-256 is unsuitable for password storage:
- Tests ~19 common passwords in milliseconds
- Successfully cracks 3 target hashes
- Shows need for key derivation functions (bcrypt, Argon2, PBKDF2)

---

## ✅ Testing & Verification

All implementations are verified against:

1. **Official Test Vectors** - From NIST FIPS 180-4
2. **Python's hashlib** - Industry-standard implementation
3. **Known Hash Values** - For common inputs like "abc"

### Test Results
```
Empty message:      e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855 ✓
"abc":              ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad ✓
"The quick...":     d7a8fbb307d7809469ca9abcb0082e4f8d5651e46d3cdb762d02d0bf37c9e592 ✓
100 'a' characters: 2816597888e4a0d3a36b82b83316ab32680eb8f00f8cd3b904d681246d285a0e ✓
```

All tests pass with 100% accuracy against reference implementations.

---

## 📦 Dependencies

### Required

- **numpy** (>=1.21.0) - For uint32 operations and mathematical functions

### Standard Library (No Installation Needed)

- **hashlib** - For verification and testing
- **warnings** - For managing overflow warnings in modular arithmetic

### Optional (Development)

- **jupyter** - For running the notebook interactively
- **pytest** - For automated testing (if extended)

### Installation
```bash
pip install -r requirements.txt
```

The implementation is intentionally minimal, relying primarily on bitwise operations rather than heavy external libraries.

---

## 📚 Background & References

### Official Documentation

- **[NIST FIPS 180-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)** - The official Secure Hash Standard specification
- **[RFC 6234](https://tools.ietf.org/html/rfc6234)** - US Secure Hash Algorithms (SHA and SHA-based HMAC and HKDF)

### Educational Resources

- **[Understanding SHA-256: A Visual Guide](https://sha256algorithm.com/)** - Interactive visualization of SHA-256
- Github CoPilot for standard coding workflow and auto-complete
- Claude AI (sonnet 4.5) for asking cryptography related questions and general educational purpouses. Used for debugging also


### Research & Advanced Topics

- **[Cryptanalysis of SHA-2 (Wikipedia)](https://en.wikipedia.org/wiki/SHA-2#Cryptanalysis_and_validation)** - Current attack research
- **[Mining Bitcoin with Pencil and Paper](http://www.righto.com/2014/09/mining-bitcoin-with-pencil-and-paper.html)** - Ken Shirriff's manual SHA-256 computation
- **[NIST Post-Quantum Cryptography](https://csrc.nist.gov/projects/post-quantum-cryptography)** - Quantum resistance considerations

### Comparative Algorithms

- **[Cryptography Hash Method MD2](https://medium.com/@nickthecrypt/cryptography-hash-method-md2-message-digest-2-step-by-step-explanation-made-easy-with-python-10faa2e35e85)** - Nick The Crypt's MD2 explanation
- **[How SHA-256 Works (Medium)](https://medium.com/@bajrang1081siyag/how-sha-256-works-4951088ab9f8)** - Accessible introduction

---

## 🔐 Security Considerations

### ⚠️ Educational Purpose Only

This implementation is for **educational purposes** to understand SHA-256 internals. For production use:

- **Use standard libraries** (`hashlib.sha256()` in Python)
- Never implement your own crypto for production systems
- Standard implementations are optimized, audited, and battle-tested

### Password Storage

**DO NOT use raw SHA-256 for passwords!**

The dictionary attack demonstration shows why:
- SHA-256 is too fast (~100M+ hashes/second on modern hardware)
- Attackers can test billions of password guesses quickly
- No built-in protection against rainbow tables

**Instead, use:**
- **bcrypt** - Adaptive cost factor, salt included
- **Argon2** - Winner of Password Hashing Competition
- **PBKDF2** - NIST recommended for key derivation
- **scrypt** - Memory-hard function

Example with bcrypt:
```python
import bcrypt

# Hashing
password = b"secure_password"
hashed = bcrypt.hashpw(password, bcrypt.gensalt())

# Verification
if bcrypt.checkpw(password, hashed):
    print("Password matches!")
```

### Known Limitations

- **Speed**: This Python implementation is ~1000x slower than optimized C implementations
- **Side-channel attacks**: Not protected against timing attacks or power analysis
- **No hardware acceleration**: Modern CPUs have SHA extensions (Intel SHA-NI) for 10x+ speedup

---

## 🚀 Future Improvements

Potential extensions to this project:

- [ ] Implement HMAC-SHA256 (keyed hash for authentication)
- [ ] Add SHA-512 variant (64-bit operations)
- [ ] Merkle tree implementation for file verification
- [ ] Performance benchmarking suite
- [ ] WebAssembly compilation for browser usage
- [ ] Visualization tool for hash computation steps
- [ ] GPU acceleration experiments
- [ ] Comparison with other hash functions (SHA-3, BLAKE2)

---

## 🙏 Acknowledgments

### Educational Institutions

- **Atlantic Technological University** - Computational Theory Module
- **Lecturer** - Ian McLoughlin 
- **NIST** - For publishing the Secure Hash Standard

---

## 📬 Contact

For questions, feedback, or collaboration:

- **GitHub**: [@JakubKopras6](https://github.com/JakubKopras6)
- **Email**: G00403547@atu.ie
- **LinkedIn**: [Jakub Kopras](https://www.linkedin.com/in/jakub-kopras-86718a211/)

---

## 📊 Project Statistics

- **Lines of Code**: ~500+ (including documentation)
- **Implementation Time**: Full semester project
- **Test Coverage**: 100% (all functions verified)
- **Documentation**: Extensive inline comments and markdown explanations

---

<div align="center">


[⬆ Back to Top](#computational-theory-Year-4-sha-256)

</div>





