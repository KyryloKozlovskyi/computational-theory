# Computational Theory

This repository contains my submission for the Computational Theory assessment for Winter 2025/26. The work focuses on implementing core components of the SHA-256 hashing algorithm as specified in the Secure Hash Standard (FIPS 180-4).

## Project Overview

This assessment required implementing and analyzing the mathematical and computational foundations of SHA-256 using Python and NumPy. All work is completed in a single Jupyter notebook called `problems.ipynb` that includes five problems covering different aspects of cryptographic hashing.

The project demonstrates understanding of:
- Binary operations and bitwise logic
- Prime-based constant generation
- Cryptographic padding schemes
- Message block processing
- Hash compression functions
- Password security and dictionary attacks

Beyond just solving the problems, the notebook includes explanations of the theory behind each concept, test cases for verification, and references to relevant standards and research.

## Repository Structure

The repository is organized simply with only the essential files:

```
computational-theory/
├── .gitignore
├── LICENSE
├── README.md
├── problems.ipynb
└── requirements.txt
```

- **problems.ipynb**: Main notebook containing all five problems with implementations, explanations, and tests
- **README.md**: This file, providing an overview and setup instructions
- **.gitignore**: Specifies files to exclude from version control
- **requirements.txt**: Lists Python dependencies (numpy)
- **LICENSE**: MIT license for the project

## How to Run

To run the notebook, you'll need to clone this repository and set up a Python environment with the required dependencies.

### Clone the Repository

Using HTTPS:
```bash
git clone https://github.com/KyryloKozlovskyi/computational-theory.git
cd computational-theory
```

Or using SSH:
```bash
git clone git@github.com:KyryloKozlovskyi/computational-theory.git
cd computational-theory
```

### Set Up Python Environment

It's recommended to use a virtual environment to keep dependencies isolated:

```bash
python3 -m venv venv
source venv/bin/activate        # On macOS/Linux
# or
venv\Scripts\activate           # On Windows
```

### Install Dependencies

Install the required packages:
```bash
pip install -r requirements.txt
```

Or install manually:
```bash
pip install numpy
```

Note: `hashlib` is part of Python's standard library and doesn't need separate installation.

### Run the Notebook

You can open the notebook using Jupyter:

```bash
jupyter notebook problems.ipynb
```

Or if you prefer JupyterLab:
```bash
jupyter lab problems.ipynb
```

Make sure to run the cells in order, starting with the imports at the top. The notebook is structured so each problem builds on previous work.

## Dependencies

This project uses only standard Python libraries plus NumPy:

- **Python 3.8+**: Required for running the code
- **NumPy**: Used for 32-bit integer operations and array handling
- **hashlib**: Built-in Python library for SHA-256 implementation in Problem 5

All dependencies are listed in `requirements.txt` for easy installation.

## Problems Overview

The notebook contains five problems that progressively build up to a complete SHA-256 implementation:

**Problem 1: Hash Functions**
Implements the seven core bitwise functions used in SHA-256 (Parity, Ch, Maj, Sigma0, Sigma1, sigma0, sigma1). Each function is tested with known inputs.

**Problem 2: Constants**
Generates the 64 K constants used in SHA-256 from the cube roots of the first 64 prime numbers. Includes verification against the official FIPS values.

**Problem 3: Padding**
Implements the message padding algorithm specified in FIPS 180-4, including proper handling of message length encoding and block boundaries.

**Problem 4: Hashing**
Implements the core SHA-256 compression function that processes message blocks and updates the hash state. Also includes a complete SHA-256 pipeline.

**Problem 5: Passwords**
Uses the SHA-256 implementation to crack three password hashes through dictionary attacks. Discusses why unsalted SHA-256 is insufficient for password storage and explores better alternatives like bcrypt, scrypt, and Argon2.

## Key Concepts

The notebook explains several important computational concepts that are fundamental to understanding SHA-256:

### 32-Bit Words and Modular Arithmetic

SHA-256 operates on 32-bit unsigned integers, with all arithmetic done modulo 2³². This means values wrap around when they exceed the maximum 32-bit value. NumPy's `uint32` type handles this automatically.

### Bitwise Operations

The algorithm relies heavily on bitwise operations (AND, OR, XOR, NOT) and bit rotations. These operations mix the message bits in ways that make the hash function cryptographically secure.

### Message Padding

Messages must be padded to a multiple of 512 bits before hashing. The padding scheme includes the original message length encoded as a 64-bit integer, which prevents certain types of attacks.

### Compression Function

The hash compression function processes each 512-bit block through 64 rounds of mixing, using the K constants and various bitwise functions to update the internal state.

## Testing and Verification

Each problem includes test cases that verify correctness:

- Functions are tested against known correct outputs
- SHA-256 results are compared with official test vectors from RFC 6234
- Password hashes are validated against expected values
- Edge cases like empty messages are tested

All tests pass, confirming the implementation matches the FIPS 180-4 specification.

## References

The work is based on official standards and academic sources:

### Primary Sources
- [FIPS 180-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf): Secure Hash Standard specification
- [RFC 6234](https://datatracker.ietf.org/doc/html/rfc6234): SHA test vectors

### Additional Resources
- [NumPy Documentation](https://numpy.org/doc/): For array operations
- [Python hashlib](https://docs.python.org/3/library/hashlib.html): Standard library documentation
- [OWASP Password Storage](https://cheatsheetsec.org/cheatsheets/Password_Storage_Cheat_Sheet.html): Security best practices
- Various Wikipedia articles on cryptographic concepts (bcrypt, scrypt, Argon2, UTF-8)

Additional references specific to each problem are included in the notebook itself.

## Development Notes

The notebook was developed incrementally with regular commits showing the progression of work. The commit history demonstrates:

- Initial implementations of basic functions
- Adding comprehensive docstrings and comments
- Refining test cases
- Adding explanatory markdown sections
- Iterative improvements based on testing

This follows the assessment requirement for steady, consistent progress rather than last-minute completion.

## Assessment Criteria

This submission addresses all five assessment categories:

**Presentation**: Clear organization with logical structure, comprehensive README, and well-formatted notebook

**Research**: Citations of FIPS 180-4, RFCs, and academic sources with context for how they inform the work

**Documentation**: Detailed explanations in markdown cells, comprehensive docstrings, inline comments, introduction and conclusion sections

**Development**: Clean, efficient code following Python conventions, proper use of data structures, comprehensive testing

**Consistency**: Regular commits throughout the development period showing incremental progress

## Author

Kyrylo Kozlovskyi  
G00387873@atu.ie  
Atlantic Technological University

## License

This project is licensed under the MIT License - see the LICENSE file for details.
