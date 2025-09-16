# Key Encapsulation Mechanism (KEM)

A fundamental cryptographic primitive for secure key establishment, especially crucial in the era of quantum computing.

## Table of Contents

1.  [Overview](#overview)
2.  [How It Works](#how-it-works)
    *   [Algorithms](#algorithms)
    *   [Flow Diagram](#flow-diagram)
3.  [KEM vs. Traditional Key Exchange](#kem-vs-traditional-key-exchange)
4.  [Security: IND-CCA2](#security-ind-cca2)
5.  [Example in Practice (TLS 1.3)](#example-in-practice-tls-13)
6.  [Types of KEMs](#types-of-kems)
    *   [Classical](#classical)
    *   [Post-Quantum (NIST Selected)](#post-quantum-nist-selected)
7.  [Why Use a KEM?](#why-use-a-kem)
8.  [Pseudocode Example](#pseudocode-example)
9.  [Further Reading](#further-reading)

## Overview

A **Key Encapsulation Mechanism (KEM)** is a cryptographic algorithm used to securely establish a shared secret key between two parties. Unlike interactive key exchange protocols (e.g., Diffie-Hellman), a KEM is **non-interactive**. One party can generate the shared secret using the other party's public key alone, without any back-and-forth communication. This makes it ideal for scenarios like encrypting data for a server.

## How It Works

A KEM is defined by three probabilistic polynomial-time algorithms:

### Algorithms

1.  **Key Generation (`KeyGen()` → (pk, sk))**
    *   Generates a matched public key (`pk`) and secret key (`sk`).

2.  **Encapsulation (`Encaps(pk) → (c, K)`)**
    *   Takes a public key (`pk`) as input.
    *   Generates a random shared secret `K` and an "encapsulation" of it, called the ciphertext `c`.
    *   The sender **outputs `(ciphertext c, shared secret K)`**. They send `c` to the recipient and use `K` for symmetric encryption.

3.  **Decapsulation (`Decaps(sk, c) → K`)**
    *   Takes the secret key (`sk`) and a ciphertext (`c`) as input.
    *   **Outputs the shared secret `K`** (or a special error symbol `⊥` if the ciphertext is invalid).
    *   The recipient recovers the same `K` that the sender generated.

### Flow Diagram

```mermaid
sequenceDiagram
    participant Sender
    participant Recipient

    Note over Recipient: KeyGen()
    Recipient->>Recipient: (pk, sk)
    Recipient->>Sender: pk

    Note over Sender: Encaps(pk)
    Sender->>Sender: (c, K)
    Sender->>Recipient: c

    Note over Recipient: Decaps(sk, c)
    Recipient->>Recipient: K

    Note over Sender, Recipient: Both now share secret K<br>for symmetric encryption.
