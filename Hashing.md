# 🔐 Hashing in Cryptography

## 1️⃣ Introduction

**Hashing** is a process used in cryptography to convert data (like a file, password, or message) into a fixed-size string of characters, called a **hash** or **digest**. Hashing is commonly used to check data integrity, store passwords securely, and support many security systems.

Hashing is not encryption. Unlike encryption, hashed data **cannot be reversed back** to its original form.

---

## 2️⃣ Why We Use Hashing ?

* **🔍 Data Integrity** – Detects if files or messages have been changed.
* **🔑 Password Security** – Passwords are stored as hashes instead of plaintext.
* **✍️ Digital Signatures** – Hashes are used in signing messages for authenticity.
* **⚡ Efficient Lookup** – Hashes help with fast data comparison and indexing.
* **⛓️ Blockchain** – Blocks are linked using cryptographic hashes.
* **📦 Software Verification** – Ensures downloaded files are not tampered.

---

## 3️⃣ How Hash Functions Work

### Basic Flow

```
Input Data ──► Hash Function ──► Fixed-Length Hash Output
```

A **hash function** takes an input (message, file, password) and returns a fixed-length output (the hash). The output:

* Looks random.
* Changes completely even if you change just one letter in the input.
* Even a **tiny change** in the input gives a **completely different hash**.
* Always gives the same output for the same input.

Example:

* Input: `"hello"`
* Output: `2cf24dba5fb0a...` (SHA-256 hash)


---

## 4️⃣ Properties of Cryptographic Hash Functions

| Property                       | Description                                                                    |
| ------------------------------ | ------------------------------------------------------------------------------ |
| **Deterministic**              | Same input always produces same hash.                                          |
| **Preimage Resistance**        | Hard to reverse a hash to find the input.                                      |
| **Second Preimage Resistance** | Hard to find a different input with the same hash as another.                  |
| **Collision Resistance**       | Infeasible to find two inputs with the same hash.                              |
| **Avalanche Effect**           | A tiny change in input causes a drastically different hash.                    |
| **Efficiency**                 | Quick to compute, even for large files.                                        |
| **Fixed Output Size**          | No matter how big input is, output is always the same length (e.g., 256 bits). |

---

## 5️⃣ Types of Hash Functions

### 🔹 Non-Cryptographic

* Used for fast data lookup (not for security).
* Examples: `MurmurHash`, `FNV`, `CityHash`.

### 🔹 Cryptographic

* Designed for **security & integrity**.
* Examples:

  * **MD5** – Deprecated (not secure).
  * **SHA-1** – Weak; collisions found.
  * **SHA-2** – Includes SHA-224, SHA-256, SHA-384, SHA-512 (widely used).
  * **SHA-3 (Keccak)** – Newest secure standard.
  * **BLAKE2 / BLAKE3** – Fast and secure modern alternatives.

---

## 6️⃣ Hashing Algorithms: Construction Styles
- Hashing algorithms can be built in different ways. Some common styles:

   | Construction Style                   | Description                                                                           | Examples                              | Use Cases / Notes                                                             |
   | ------------------------------------ | ------------------------------------------------------------------------------------- | ------------------------------------- | ----------------------------------------------------------------------------- |
   | **Merkle–Damgård**                   | Splits input into fixed-size blocks, processes with a compression function.           | MD5, SHA-1, SHA-2                     | Classic design, vulnerable to length extension attacks if not used with care. |
   | **Sponge Construction**              | Absorbs input bit-by-bit into an internal state, then "squeezes" output.              | SHA-3 (Keccak)                        | Flexible output length, strong security, resistant to extension attacks.      |
   | **HAIFA (Hash Iterative Framework)** | Enhancement of Merkle–Damgård with additional input parameters (e.g., salt, counter). | BLAKE, BLAKE2                         | Adds resistance to some known weaknesses of Merkle–Damgård.                   |
   | **Wide-Pipe**                        | Uses larger internal state than the output size to prevent collisions.                | Whirlpool, SHA-512/256                | Offers better collision resistance.                                           |
   | **Tree Hashing / Merkle Trees**      | Splits data into chunks, hashes them, and combines using a binary tree structure.     | BLAKE3, Parallel SHA, Merkle Trees    | Enables parallel processing, ideal for large data sets or blockchain.         |
   | **Feedforward Construction**         | Combines the input with the output of the compression function at each step.          | RIPEMD, Tiger                         | Adds security layer to Merkle–Damgård.                                        |
   | **Domain Separation Construction**   | Processes different parts of input (like key, data, salt) separately.                 | HMAC, TupleHash                       | Prevents cross-domain collision issues.                                       |
   | **Random Oracle Model-based**        | Theoretical model assuming hash acts like a black-box random function.                | Cryptographic proofs, protocol design | Not practical implementation but used in security proofs.                     |
   | **Permutation-Based Hashing**        | Uses permutations (rather than compression) to transform data.                        | Keccak (SHA-3), Gimli                 | Often used in lightweight and sponge constructions.                           |
   | **Compression Function Chaining**    | Chaining a core compression function over input blocks.                               | MD4 family (MD5, SHA-1)               | Foundation of early hash designs.                                             |
   | **Rate + Capacity Model (Sponge)**   | Two-phase: absorb input at “rate”, output based on “capacity”.                        | SHA-3, KangarooTwelve                 | Enables variable-length hash output and high flexibility.                     |

---

## 7️⃣ Hash-Based Cryptographic Primitives

- **Hash-Based Cryptographic Primitives** are cryptographic techniques that use the core properties of hash functions to provide secure features like authentication, key generation, and digital signatures. They're efficient, often quantum-resistant, and form the backbone of modern cryptographic protocols.

- Instead of relying on complex math (like prime factorization or elliptic curves), these primitives achieve security mainly through the properties of **cryptographic hash functions** — especially collision resistance, preimage resistance, and determinism.

---

### 🧩 Key Characteristics

   * Based on **well-established hash functions** (e.g., SHA-2, SHA-3, BLAKE2).
   * Often more **efficient and post-quantum secure** than traditional cryptographic primitives.
   * Generally **simpler to implement** and audit.
   * Can be **stateless** and avoid key reuse issues.

---

### 🛠️ Examples of Hash-Based Cryptographic Primitives

  | Primitive                                         | Purpose                                 | Description                                                                                            |
  | ------------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------ |
  | **HMAC** (Hash-based Message Authentication Code) | **Authentication + Integrity**          | Combines a secret key with a message using a hash function. Used in SSL/TLS, APIs, JWT.                |
  | **HKDF** (HMAC-based Key Derivation Function)     | **Key Derivation**                      | Generates multiple cryptographic keys from a single shared secret (e.g., in TLS).                      |
  | **Hash-Based Digital Signatures**                 | **Authentication**                      | Used in post-quantum cryptography (e.g., LMS, XMSS, SPHINCS+). Strong against quantum attacks.         |
  | **Merkle Trees**                                  | **Data Verification**                   | A tree of hashes used to verify large data sets (e.g., in blockchains).                                |
  | **Hash Chains**                                   | **One-Time Passwords / Authentication** | A sequence of hashes used once in order (e.g., Lamport signatures, OTP systems).                       |
  | **Commitment Schemes**                            | **Hiding and Binding**                  | Hash is used to commit to a value without revealing it. Later, the value can be revealed and verified. |

---

### 🔒 Security Based on Hash Properties

These primitives **do not require asymmetric key cryptography** and their security relies on the hardness of:

* Finding a **preimage** (i.e., input from a given hash).
* Finding **collisions** (two inputs with same hash).
* Ensuring the **integrity and authenticity** of data.

---

### ✅ Advantages

* **Post-quantum secure** (unlike RSA, ECC).
* Often faster than traditional cryptographic methods.
* Easier to implement securely.
* Minimal reliance on complex algebra or number theory.

---

## 8️⃣ Password Hashing (Important Distinction)

Hashing passwords is different from regular hashing.

* **Regular Hashing** (e.g., SHA-256) is **not safe** for passwords.
* Use **slow, salted** hashing algorithms for passwords:

  * **Salt**: A random value added to the password before hashing to prevent attackers from using precomputed hash tables.
  * **Slow algorithms**: Make brute-force attacks harder.

| Algorithm  | Key Feature                                                  |
| ---------- | ------------------------------------------------------------ |
| **bcrypt** | Slows down attackers; salt built-in.                         |
| **scrypt** | Memory-hard, resists GPU/ASIC attacks.                       |
| **Argon2** | Winner of Password Hashing Competition (PHC); highly secure. |

---

## 9️⃣ Applications of Hashing

| Field                       | Use Case                                          |
| --------------------------- | ------------------------------------------------- |
| **Authentication**          | Password verification, OTP generation.            |
| **Digital Signatures**      | Secure message verification.                      |
| **Blockchain**              | Blocks linked using SHA-256 or Keccak (Ethereum). |
| **Software Updates**        | File verification using checksums.                |
| **Network Security**        | Packet integrity checks.                          |
| **Certificate Authorities** | Fingerprinting public keys.                       |

---

## 🔟 Security Considerations
* **Collision Attacks**: If two inputs have the same hash, it can break security.

  * MD5 and SHA-1 are **no longer secure**.
* **Brute Force**: If the hash function is too fast, attackers can try many guesses quickly.

  * Use slow hashing for passwords.
* **Rainbow Tables**: Precomputed hash lists used for cracking.

  * Prevented by using **salts**.


* Use **HMAC** for data authentication.
* Choose hash functions with **peer-reviewed security**.

---

## 🔐 Salting in Password Hashing

### ➕ What is a Salt?

A **salt** is a random value added to the password **before** hashing.

### 🔁 Salted Hashing Flow

```
Input Password + Salt ──► Hash Function ──► Store (Salt, Hash)
```

### ⚠️ Without Salt

* Same password → Same hash → Easy for attackers to guess.

### ✅ With Salt

* Same password → Different hash → Protects against rainbow tables.

---

## 🌶️ Peppering (Extra Security)

A **pepper** is a **secret key** stored separately (not in the database) that’s also added before hashing.

* Example: `Hash(password + salt + pepper)`
* **Adds protection even if database is compromised.**

---

## ⚠️ Hash Collisions (In Detail)

* A **collision** happens when two **different inputs** produce the **same hash**.
* Cryptographic hash functions should make collisions **extremely rare**.
* MD5 and SHA-1 are known to have collisions — avoid them.

---

## 📷 Hashing in Digital Forensics

* Hashing is used to **verify data integrity** in court.
* When investigators clone a hard drive, they compare hashes to confirm **no tampering** occurred.
* Common tools: `SHA-256`, `MD5` (only for validation, not security).

---

## 🧠 Hashing in Zero-Knowledge Proofs

* In **Zero-Knowledge Proofs (ZKPs)**, hashes can help prove that a party knows a secret **without revealing it**.
* Used in **Zcash**, **zk-SNARKs**, and **anonymous authentication** systems.

---

## 🔁 Avalanche Effect

| Input     | Hash (SHA-256)                                                     |
| --------- | ------------------------------------------------------------------ |
| `"Hello"` | `185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969` |
| `"hello"` | `2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824` |

Even a small change (capital "H" → lowercase "h") results in a completely different hash.

---

## 🧾 Message Digest, Integrity & Confidentiality

### 📌 What is a Message Digest?

* A fixed-size output representing the data.
* Ensures **data integrity** (detects changes).

### 🔍 Integrity vs. Confidentiality

| Property            | Definition                                                         |
| ------------------- | ------------------------------------------------------------------ |
| **Integrity**       | Ensures data hasn’t been changed (via hash).                       |
| **Confidentiality** | Ensures data is hidden from unauthorized parties (via encryption). |

✅ **Hash → Integrity**

✅ **Encryption → Confidentiality**

✅ **Hash + Encrypt → Both**

---

## 📤 Sender → Receiver Integrity Flow

1. Sender hashes the message → gets H.
2. Sends both the message + H.
3. Receiver hashes the received message → gets H’.
4. Compares H and H’.

   * If they match → ✅ Message is intact.
   * If not → ⚠️ Message was changed.

---

## 📚 Summary

| Feature       | Description                                                        |
| ------------- | ------------------------------------------------------------------ |
| **Hashing**   | One-way transformation from data to fixed output.                  |
| **Used For**  | Password storage, data integrity, digital signatures, blockchains. |
| **Don't Use** | MD5 and SHA-1 — too weak.                                          |
| **Do Use**    | SHA-2, SHA-3, bcrypt, Argon2.                                      |
|**Important Concepts** | Avalanche effect, collision resistance, salted hashes. |
| **Extra Protection** | Use peppering, HMAC, and proper key management. |

---
