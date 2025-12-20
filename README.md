# Computational Theory

This repository contains my submission for the **Computational Theory** module assessment. The work focuses on implementing and understanding the core components of the **SHA-256** cryptographic hash function as specified in [FIPS 180-4, the Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf).

## About This Project

The assessment required implementing the mathematical and computational foundations of SHA-256 from scratch using Python and NumPy. Rather than simply calling library functions, I built the algorithm piece by piece to demonstrate understanding of how cryptographic hash functions work internally.

All work is completed in a single Jupyter notebook ([`problems.ipynb`](problems.ipynb)) containing five interconnected problems that progressively build up to a complete SHA-256 implementation, followed by a practical security analysis.

## Repository Structure

```
computational-theory/
├── .gitignore          # Files excluded from version control
├── README.md           # This file
├── problems.ipynb      # Main notebook with all solutions
└── requirements.txt    # Python dependencies
```

## Notebook Structure

The notebook follows a clear narrative structure organized as follows:

```
problems.ipynb
│
├── Title & Introduction
│   ├── # Computational Theory Assessment (Title)
│   └── ## Introduction (Overview, goals, and approach)
│
├── Imports
│   └── numpy, hashlib
│
├── ## Problem 1: Binary Words and Operations
│   ├── Problem Description & Understanding
│   ├── Approach & Theory (bitwise operations explained)
│   ├── Parity(x, y, z) — implementation + tests
│   ├── Ch(x, y, z) — implementation + tests
│   ├── Maj(x, y, z) — implementation + tests
│   ├── Sigma0(x) — implementation + tests
│   ├── Sigma1(x) — implementation + tests
│   ├── sigma0(x) — implementation + tests
│   └── sigma1(x) — implementation + tests
│
├── ## Problem 2: Fractional Parts of Cube Roots
│   ├── Problem Description & Approach
│   ├── Mathematical Background (cube roots, primes)
│   ├── primes(n) — prime number generator + tests
│   ├── Newton-Raphson Discussion
│   ├── fractional_part_cube_root() — implementation
│   ├── generate_k_constants() — K constant generation
│   ├── Display results in hexadecimal
│   └── Verification against FIPS 180-4 standard
│
├── ## Problem 3: Padding
│   ├── Problem Description & Requirements
│   ├── Padding Theory (FIPS 180-4 Sections 5.1.1, 5.2.1)
│   ├── Step-by-step padding algorithm
│   ├── block_parse(msg) — generator function
│   └── Tests (various message lengths, edge cases)
│
├── ## Problem 4: Hashes
│   ├── Problem Description & Function Signature
│   ├── SHA-256 Compression Theory
│   ├── Message Schedule Explanation
│   ├── 64-Round Mixing Process
│   ├── hash(current, block) — compression function
│   ├── sha256(message) — complete pipeline
│   └── Tests against RFC 6234 test vectors
│
├── ## Problem 5: Passwords
│   ├── Problem Description & Target Hashes
│   ├── Dictionary Attack Theory
│   ├── Why SHA-256 Alone is Insufficient
│   ├── Modern Password Security (salting, KDFs)
│   ├── sha256_utf8() — helper function
│   ├── Dictionary attack implementation
│   ├── Results verification
│   └── Security recommendations discussion
│
└── ## Conclusion
    ├── Key Achievements & Insights
    ├── Technical Challenges & Solutions
    ├── Broader Context & Applications
    └── Future Directions
```

Each problem section follows a consistent pattern:

1. **Problem Description** — What needs to be solved
2. **Understanding/Approach** — My interpretation and strategy
3. **Theory/Background** — Mathematical and cryptographic foundations
4. **Implementation** — Well-documented Python code with docstrings
5. **Testing** — Verification against known values and edge cases

## Quick Start

### Prerequisites

- Python 3.8 or higher
- Git

### Clone the Repository

```bash
git clone https://github.com/KyryloKozlovskyi/computational-theory.git
cd computational-theory
```

### Set Up Environment (Recommended)

```bash
python3 -m venv venv
source venv/bin/activate    # On macOS/Linux
# or
venv\Scripts\activate       # On Windows
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run the Notebook

```bash
jupyter notebook problems.ipynb
```

Or using JupyterLab:

```bash
jupyter lab problems.ipynb
```

**Note:** Run cells in order from top to bottom, as later problems depend on functions defined in earlier ones.

## Problems Overview

The notebook addresses five problems from the [Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf):

### Problem 1: Binary Words and Operations

Implements the seven core bitwise functions used in SHA-256's compression function:

| Function          | Description                                             |
| ----------------- | ------------------------------------------------------- |
| `Parity(x, y, z)` | XOR-based parity function                               |
| `Ch(x, y, z)`     | Choose function - selects bits from y or z based on x   |
| `Maj(x, y, z)`    | Majority function - returns majority bit of x, y, z     |
| `Sigma0(x)`       | Upper-case sigma with rotations (2, 13, 22)             |
| `Sigma1(x)`       | Upper-case sigma with rotations (6, 11, 25)             |
| `sigma0(x)`       | Lower-case sigma with rotations (7, 18) and shift (3)   |
| `sigma1(x)`       | Lower-case sigma with rotations (17, 19) and shift (10) |

Each function uses NumPy's `uint32` type to ensure proper 32-bit arithmetic with modular wraparound.

### Problem 2: Fractional Parts of Cube Roots

Generates the 64 **K constants** used in SHA-256 by:

1. Finding the first 64 prime numbers
2. Computing the cube root of each prime
3. Extracting the first 32 bits of each fractional part

This "nothing up my sleeve" approach proves the constants aren't arbitrary, they're mathematically derived from well-defined operations, ensuring transparency in the algorithm's design.

### Problem 3: Padding

Implements a generator function `block_parse(msg)` that:

- Accepts a bytes object as input
- Pads messages according to [FIPS 180-4 Sections 5.1.1 and 5.2.1](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)
- Yields 512-bit (64-byte) blocks for processing
- Handles edge cases where padding spans multiple blocks

The padding scheme appends a `1` bit, followed by zeros, and finally the original message length as a 64-bit big-endian integer.

### Problem 4: Hashes

Implements the `hash(current, block)` function following [FIPS 180-4 Section 6.2.2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf):

1. Prepares a 64-word message schedule from the 512-bit block
2. Initializes eight working variables from the current hash state
3. Performs 64 rounds of mixing using the functions from Problem 1 and constants from Problem 2
4. Computes the intermediate hash value through feed-forward addition

Also includes a complete `sha256(message)` function that chains all components together, verified against [RFC 6234](https://datatracker.ietf.org/doc/html/rfc6234) test vectors.

### Problem 5: Passwords

Cracks three SHA-256 password hashes through dictionary attack:

| Hash                                                               | Password Found |
| ------------------------------------------------------------------ | -------------- |
| `5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8` | `password`     |
| `873ac9ffea4dd04fa719e8920cd6938f0c23cd678af330939cff53c3d2855f34` | `cheese`       |
| `b03ddf3ca2e714a6548e7495e2a03f5e824eaac9837cd7f159c67b90fb4b7342` | `P@ssw0rd`     |

Discusses why unsalted SHA-256 is insufficient for password storage and explores modern alternatives:

- **Salting**: Adding unique random data to each password
- **Key Derivation Functions**: [bcrypt](https://en.wikipedia.org/wiki/Bcrypt), [scrypt](https://en.wikipedia.org/wiki/Scrypt), [Argon2](https://en.wikipedia.org/wiki/Argon2)
- **Iteration counts**: Making hash computation intentionally slow

## Dependencies

| Package                                                   | Purpose                                              |
| --------------------------------------------------------- | ---------------------------------------------------- |
| [NumPy](https://numpy.org/)                               | 32-bit integer operations and array handling         |
| [hashlib](https://docs.python.org/3/library/hashlib.html) | SHA-256 verification in Problem 5 (standard library) |

## Key Concepts Demonstrated

### 32-Bit Modular Arithmetic

SHA-256 operates on 32-bit unsigned integers with all arithmetic done modulo 2³². NumPy's `uint32` type handles overflow correctly.

### Bitwise Operations

The algorithm relies on AND, OR, XOR, NOT operations plus circular rotations to mix message bits thoroughly.

### Avalanche Effect

Small input changes produce completely different outputs, demonstrated through testing where single-bit changes alter approximately 50% of output bits.

### Cryptographic Transparency

The "nothing up my sleeve" constant generation proves no hidden backdoors exist in the algorithm's design.

## Testing

Each problem includes test cases verified against:

- Known correct outputs from the FIPS 180-4 standard
- [RFC 6234](https://datatracker.ietf.org/doc/html/rfc6234) SHA-256 test vectors
- Edge cases (empty messages, messages requiring multi-block padding)

## References

All references used throughout the notebook are listed below, organized by category.

### Primary Standards & Specifications

- [FIPS 180-4: Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf) - Official NIST specification for SHA-256
- [RFC 6234: US Secure Hash Algorithms](https://datatracker.ietf.org/doc/html/rfc6234) - SHA test vectors and implementation examples
- [NIST Cryptographic Standards and Guidelines](https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines) - NIST cryptography program

### Python & NumPy Documentation

- [NumPy Documentation](https://numpy.org/doc/) - Array operations and data types
- [NumPy uint32](https://numpy.org/doc/stable/reference/arrays.scalars.html#numpy.uint32) - 32-bit unsigned integer type
- [NumPy cbrt](https://numpy.org/doc/stable/reference/generated/numpy.cbrt.html) - Cube root function
- [Python hashlib](https://docs.python.org/3/library/hashlib.html) - Standard library hash functions
- [Python bytes](https://docs.python.org/3/library/stdtypes.html#bytes) - Binary data type documentation
- [PEP 8](https://peps.python.org/pep-0008/) - Python style guide

### Tutorials & Educational Resources

- [Real Python: Bitwise Operators](https://realpython.com/python-bitwise-operators/) - Understanding bitwise operations
- [Real Python: Generators](https://realpython.com/introduction-to-python-generators/) - Generator function patterns
- [Real Python: Python Bytes](https://realpython.com/python-bytes/) - Working with binary data
- [GeeksforGeeks: Bitwise Operators](https://www.geeksforgeeks.org/python-bitwise-operators/) - Bitwise operation examples
- [GeeksforGeeks: Newton-Raphson Method](https://www.geeksforgeeks.org/engineering-mathematics/newton-raphson-method/) - Numerical methods
- [Computerphile: SHA-256](https://www.youtube.com/watch?v=DMtFhACPnTY) - Visual explanation of SHA-256
- [Stack Overflow: Binary Rotation in Python](https://stackoverflow.com/questions/5832982/how-to-get-the-logical-right-binary-rotation-in-python) - Rotation implementation

### Cryptography & Security

- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) - Password security best practices
- [Hashcat Benchmarks](https://gist.github.com/epixoip/ace60d09981be09544fdd35005051505) - GPU hashing performance
- [NordPass Common Passwords](https://nordpass.com/most-common-passwords-list/) - Annual password analysis

### Wikipedia References

- [SHA-2](https://en.wikipedia.org/wiki/SHA-2) - SHA-256 algorithm overview and cryptanalysis
- [Prime Numbers](https://en.wikipedia.org/wiki/Prime_number) - Mathematical foundations
- [Trial Division](https://en.wikipedia.org/wiki/Trial_division) - Prime number generation algorithm
- [Newton's Method](https://en.wikipedia.org/wiki/Newton%27s_method) - Numerical root finding
- [Merkle-Damgård Construction](https://en.wikipedia.org/wiki/Merkle%E2%80%93Damg%C3%A5rd_construction) - Hash function design
- [Avalanche Effect](https://en.wikipedia.org/wiki/Avalanche_effect#In_cryptography) - Cryptographic property
- [Length Extension Attack](https://en.wikipedia.org/wiki/Length_extension_attack) - Security vulnerability
- [Endianness](https://en.wikipedia.org/wiki/Endianness) - Byte ordering
- [UTF-8](https://en.wikipedia.org/wiki/UTF-8) - Character encoding
- [RockYou Breach](https://en.wikipedia.org/wiki/RockYou) - Password breach history
- [bcrypt](https://en.wikipedia.org/wiki/Bcrypt) - Password hashing function
- [scrypt](https://en.wikipedia.org/wiki/Scrypt) - Memory-hard key derivation
- [Argon2](https://en.wikipedia.org/wiki/Argon2) - Modern password hashing (PHC winner)

## Author

**Kyrylo Kozlovskyi**  
G00425385@atu.ie

---

AI assistance tools (GitHub Copilot with Claude Sonnet 4.5, Claude Opus 4.5, and Claude.ai) were used to help generate test cases, draft docstrings, and write code comments.

---
