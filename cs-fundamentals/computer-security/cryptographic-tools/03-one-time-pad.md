# 03. One-Time Pad (OTP)

## Section Plan
1. Definition
2. Principles of Security
3. Step-by-Step Logic
4. Worked Example
5. Complexity
6. Weaknesses & Practical Issues
7. Metaphor
8. Summary & Takeaways

---

## 1. Definition
- **One-Time Pad (OTP)** is a cipher where:
  - The key is a **random sequence** of bits (or letters).
  - The key length is equal to the plaintext length.
  - Each plaintext symbol is combined with the corresponding key symbol (often via XOR).
- **Claim**: If used correctly, OTP provides **perfect secrecy** — the ciphertext reveals no information about the plaintext.

---

## 2. Principles of Security
For OTP to be **absolutely secure**, three conditions must be met:
1. **Truly random key**: Generated from an unpredictable source.
2. **Key as long as the message**: No reuse, no shorter keys.
3. **Key never reused**: Each key used only once, then discarded.
4. **Key secrecy**: Must be securely shared between sender and receiver.

*Violation of any condition = insecurity.*

---

## 3. Step-by-Step Logic

### Sub‑plan
1) Inputs & Preconditions  
2) Bitwise OTP (XOR) — Encrypt/Decrypt  
3) Alphabetic OTP (mod 26) — Encrypt/Decrypt  
4) Self‑Checks (Round‑trip tests)  
5) Edge Cases & Policies  
6) Security Checklist

---

### 3.2 Bitwise OTP (XOR) — Encrypt/Decrypt
Encryption rule (per bit): `ci = pi XOR ki`  
Decryption rule (per bit): `pi = ci XOR ki`

```text
function otp_xor_encrypt(P_bits, K_bits):
    assert length(P_bits) == length(K_bits)
    C_bits = empty
    for i from 1 to length(P_bits):
        C_bits[i] = P_bits[i] XOR K_bits[i]
    return C_bits

function otp_xor_decrypt(C_bits, K_bits):
    assert length(C_bits) == length(K_bits)
    P_bits = empty
    for i from 1 to length(C_bits):
        P_bits[i] = C_bits[i] XOR K_bits[i]   # XOR is its own inverse
    return P_bits

    ```

    ---

### 3.3 Alphabetic OTP (mod 26) — Encrypt/Decrypt

**Mapping Rule**  
- Convert letters to numbers: `A→0, B→1, …, Z→25`.  
- **Encrypt**: `Ci = (Pi + Ki) mod 26`  
- **Decrypt**: `Pi = (Ci - Ki) mod 26`  

---

**Encryption Pseudocode**
```text
function otp_alpha_encrypt(P_text, K_text):
    assert length(P_text) == length(K_text)
    C_text = ""
    for i in 1..length(P_text):
        Pi = map_letter_to_int(P_text[i])   # A→0 ... Z→25
        Ki = map_letter_to_int(K_text[i])
        Ci = (Pi + Ki) mod 26
        C_text += map_int_to_letter(Ci)
    return C_text

    function otp_alpha_decrypt(C_text, K_text):
    assert length(C_text) == length(K_text)
    P_text = ""
    for i in 1..length(C_text):
        Ci = map_letter_to_int(C_text[i])
        Ki = map_letter_to_int(K_text[i])
        Pi = (Ci - Ki) mod 26
        P_text += map_int_to_letter(Pi)
    return P_text
```

### Policy Notes

- Non-letters: either remove before encrypting, or carry them unchanged.
- Lowercase: normalize to uppercase, or define a parallel map.
- Key must be same length as plaintext, truly random, and never reused.

-- 

### 3.4 Self-Checks (Round-Trip Tests)
- **Involution test**:  
  `otp_xor_decrypt(otp_xor_encrypt(P, K), K) == P`  
- **Length test**:  
  `len(K) == len(P) == len(C)`  
- **Randomness test (basic)**:  
  Ciphertext bit distribution ≈ uniform (no visible bias).  

---

### 3.5 Edge Cases & Policies
- **Key length mismatch** → **abort** (never pad key deterministically).  
- **Key reuse (even once)** → **forbidden**; leaks `P1 XOR P2`.  
- **Character set**: define normalization (e.g., ASCII bytes vs A–Z only).  
- **Transport/storage**: use binary-safe encodings (e.g., Base64) for `C` if needed.  

---

### 3.6 Security Checklist
- Key is **truly random** (TRNG or cryptographically secure RNG sourcing entropy).  
- Key is **one-time** and **as long as the message**.  
- Key is **distributed via a secure channel** and **deleted after use**.  
- Implementation avoids leaving key material in logs, swap files, or crash dumps.  
- Side channels (timing, memory access patterns) do not leak key/plaintext.  


---

## 4. Worked Example

### 4.1 Example with Letters (mod 26)
- **Plaintext**: `HELLO`  
- **Key**: `XMCKL` (random, same length)  
- Convert letters A→0 … Z→25  

Step-by-step encryption:  

H (7) + X (23) = 30 → 30 mod 26 = 4 → **E**  
E (4) + M (12) = 16 → **Q**  
L (11) + C (2) = 13 → **N**  
L (11) + K (10) = 21 → **V**  
O (14) + L (11) = 25 → **Z**  


- **Ciphertext**: `EQNVZ`  
- **Decryption**: Subtract key values (mod 26) → recovers `HELLO`.

---

### 4.2 Example with Bits (XOR)
- **Plaintext (P)**: `1010 1100`  
- **Key (K)**: `0110 1011`  
- **Encryption**: `C = P XOR K` → `1100 0111`  
- **Decryption**: `C XOR K = P`  

---

### 4.3 Observations
- Ciphertext looks **completely random** if the key is truly random.  
- Without the key, **all plaintexts of equal length are equally likely** → perfect secrecy.  
- If the key were reused, XORing two ciphertexts would leak information about both plaintexts.  

---

## 5. Complexity

### 5.1 Time Complexity
- **Encryption**: `O(n)` for `n` characters (one XOR or modular addition per symbol).  
- **Decryption**: `O(n)` (same as encryption).  
- Each operation is constant time.

### 5.2 Space Complexity
- **Ciphertext**: `O(n)` (same size as plaintext).  
- **Key**: `O(n)` (must be as long as the plaintext).  
- **Total**: `O(n)` storage requirement.

### 5.3 Communication Overhead
- Sender and receiver must **share a key equal in size to the message**, which doubles storage/transmission requirements compared to ciphers with smaller keys.

### 5.4 Efficiency Note
- The OTP itself is efficient to compute (just XOR/addition).  
- The bottleneck is **key distribution and management**, not the math.

---

## 6. Weaknesses & Practical Issues

### 6.1 Key Distribution Problem
- The key must be **as long as the message**.  
- Securely exchanging such long keys is often harder than sending the message itself.  
- This makes OTP impractical for large-scale communication.

---

### 6.2 Key Reuse Danger
- If the same key is used for two messages:  
  - Let C1 = P1 XOR K  
  - Let C2 = P2 XOR K  
  - Then C1 XOR C2 = P1 XOR P2 → leaks patterns from both plaintexts.  
- Known as the **“two-time pad problem”** — instantly breaks security.

---

### 6.3 Randomness Requirement
- Key must be **truly random**, not generated by a predictable algorithm.  
- Pseudo-random keys make OTP vulnerable to cryptanalysis.  
- Hardware random number generators (TRNGs) are required, which are not always practical.

---

### 6.4 Storage & Management
- Both sender and receiver must store massive keys securely.  
- Keys must never be reused, copied carelessly, or leaked.  
- Difficult to scale when millions of messages are exchanged.

---

### 6.5 Real-World Usage
- OTP is rarely used in everyday systems because of the above limitations.  
- Still used in **high-security channels** (e.g., diplomatic and military communications) where secure couriers deliver the key material in advance.

---


## 7. Metaphor

Imagine writing your secret message on a sheet of paper.  
Then, to hide it, you cover the entire sheet with a **completely random scribble** that is exactly the same length as your message.  

- If the scribble is truly random and **only used once**, no one can tell what the original message was.  
- To an outsider, the sheet looks like **pure noise**.  
- But if you reuse the same scribble for two different messages, patterns appear and both secrets can be uncovered.  

This is why the system is called a **One-Time Pad**:  
- One-time → key used once, never again.  
- Pad → historically, keys were written on small paper pads, torn off and destroyed after use.

---

## 8. Summary & Takeaways

- The **One-Time Pad (OTP)** is the **only cipher proven to be unbreakable** if used correctly.  
- Requirements for perfect secrecy:  
  1. **Truly random key**  
  2. **Key as long as the message**  
  3. **Key used only once**  
  4. **Key kept secret**  

- **Strengths**:  
  - Provides absolute, mathematical security.  
  - Extremely simple operations (XOR or modular addition).  
  - Ciphertext looks indistinguishable from random noise.  

- **Weaknesses**:  
  - Impractical for most real-world communication due to **key distribution**.  
  - Vulnerable if keys are reused or not truly random.  
  - Requires secure storage and destruction of massive key material.  

- **Practical note**:  
  - OTP is used only in niche, high-security environments (diplomatic, military).  
  - Modern cryptography instead relies on **AES, RSA, ECC**, which are not “perfectly secure” but are **efficient and practical**.  

**Mental model**:  
Think of OTP as a **bulletproof safe** — absolutely secure, but so heavy and inconvenient that you can’t carry it around for daily use. That’s why modern systems use lighter but still very strong locks.

