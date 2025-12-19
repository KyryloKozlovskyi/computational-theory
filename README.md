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

### Primary Sources

- [FIPS 180-4: Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf) - Official NIST specification
- [RFC 6234](https://datatracker.ietf.org/doc/html/rfc6234) - SHA test vectors and examples

### Additional Resources

- [NumPy Documentation](https://numpy.org/doc/) - Array operations and data types
- [Python hashlib](https://docs.python.org/3/library/hashlib.html) - Standard library hash functions
- [PEP 8](https://peps.python.org/pep-0008/) - Python style guide
- [Real Python: Generators](https://realpython.com/introduction-to-python-generators/) - Generator function patterns

### Security Resources

- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- Wikipedia articles on [bcrypt](https://en.wikipedia.org/wiki/Bcrypt), [scrypt](https://en.wikipedia.org/wiki/Scrypt), [Argon2](https://en.wikipedia.org/wiki/Argon2)

## Author

**Kyrylo Kozlovskyi**  
G00425385@atu.ie

---
