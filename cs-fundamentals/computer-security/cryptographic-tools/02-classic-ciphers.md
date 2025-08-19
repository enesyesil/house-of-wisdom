# 02. Classic Ciphers

## Section Plan
1. Caesar Cipher
2. Substitution Cipher
3. Frequency Analysis
4. Summary & Takeaways

---

# 02. Classic Ciphers

## Section Plan
1. Caesar Cipher
2. Substitution Cipher
3. Frequency Analysis
4. Summary & Takeaways

---

## 1. Caesar Cipher

### Definition
A shift cipher where each alphabetic character is shifted by a fixed number `k` positions in the alphabet. Non-letters may be kept unchanged.

### Formula
- **Encryption**: `C = (P + k) mod 26`
- **Decryption**: `P = (C - k) mod 26`  
Where `P, C ∈ {0…25}` map A→0, B→1, …, Z→25.

### Step-by-Step Logic (Encryption)
1. Choose a shift key `k` in `[0, 25]`.  
2. For each character `ch` in the plaintext:
   - If uppercase:
     - `base = 'A'`
     - `idx = ord(ch) - ord(base)`
     - `new_idx = (idx + k) mod 26`
     - `out = chr(new_idx + ord(base))`
   - If lowercase:
     - Same, but with `base = 'a'`.
   - If non-alphabetic:
     - Copy as-is (spaces, digits, punctuation).
3. Concatenate results → ciphertext.

### Step-by-Step Logic (Decryption)
Same process, but compute:  
`new_idx = (idx - k) mod 26`

### Pseudocode
### Caesar Cipher Pseudocode

```text
function caesar_encrypt(text, k):
    out = ""
    for ch in text:
        if 'A' <= ch <= 'Z':
            base = 'A'
            idx = ord(ch) - ord(base)
            out += chr((idx + k) mod 26 + ord(base))
        else if 'a' <= ch <= 'z':
            base = 'a'
            idx = ord(ch) - ord(base)
            out += chr((idx + k) mod 26 + ord(base))
        else:
            out += ch
    return out

function caesar_decrypt(text, k):
    return caesar_encrypt(text, (26 - (k mod 26)) mod 26)
```



- **Weakness**: Only 25 possible shifts → trivial brute-force attack.  
- **Metaphor**: Like moving all the books in a library 3 shelves over. Anyone who knows the rule can undo it quickly.

---

## 2. Substitution Cipher

### Definition
A cipher where each letter of the alphabet is replaced with another letter or symbol according to a fixed mapping (called the key).

### Key (Mapping)
- Key = a **one-to-one mapping** between plaintext alphabet and ciphertext alphabet.
- Example mapping:

- Plain : ABCDEFGHIJKLMNOPQRSTUVWXYZ
- Cipher: QWERTYUIOPASDFGHJKLZXCVBNM

- Encryption: Replace each letter using the mapping.  
- Decryption: Use the **inverse mapping**.

### Step-by-Step Logic (Encryption)
1. Define a mapping `map` from plaintext → ciphertext (e.g., A→Q, B→W, …).  
2. For each character `ch` in the plaintext:
 - If `ch` is a letter, replace with `map[ch]`.  
 - If not a letter, keep unchanged.  
3. Concatenate → ciphertext.

### Step-by-Step Logic (Decryption)
1. Build the **inverse map** (ciphertext → plaintext).  
2. For each `ch` in ciphertext:
 - If `ch` is in inverse map, replace with original letter.  
 - Else copy unchanged.  
3. Concatenate → plaintext.

### Pseudocode
```text
function substitution_encrypt(text, mapping):
  out = ""
  for ch in text:
      if ch in mapping:
          out += mapping[ch]
      else:
          out += ch
  return out

function substitution_decrypt(text, mapping):
  inverse = invert(mapping)   # swap keys and values
  out = ""
  for ch in text:
      if ch in inverse:
          out += inverse[ch]
      else:
          out += ch
  return out
```
### Worked Example
- **Mapping**: A→Q, B→W, C→E, D→R, …  
- **Plaintext**: `HELLO`  
- **Encrypted**: `ITSSG`  
- **Decrypted with inverse mapping** → `HELLO`

---

### Complexity
- **Time**: `O(n)` for `n` characters.  
- **Space**: `O(1)` for fixed mapping table.

---

### Weakness
- Patterns in natural language are preserved.  
- Example: Repeated letters in plaintext → repeated ciphertext letters.  
- **Frequency analysis** breaks it by comparing ciphertext frequencies with expected English letter frequencies.

---

### Metaphor
Imagine renaming all your friends with code names:  
- Alice → Zebra  
- Bob → Whale  

Over time, people notice that “Whale” shows up a lot, and deduce it must be “Bob.”

---

## 3. Frequency Analysis

### Definition
A cryptanalysis technique that studies how often certain symbols or letters occur in ciphertext.  
The core idea: **language has predictable patterns** → exploit them to guess the mapping.

---

### Step-by-Step Logic
1. **Collect statistics**: Count how often each symbol/letter appears in the ciphertext.  
2. **Compare with known frequencies**: Match these counts against typical letter distributions in the target language.  
   - Example (English letter frequency, rough order):  
     E, T, A, O, I, N, S, H, R, D, L, C, U, M, W, F, G, Y, P, B, V, K, J, X, Q, Z.  
3. **Make substitutions**: Replace the most frequent ciphertext symbol with “E” (or “T”), the second with “T”, etc.  
4. **Refine using digrams and trigrams**: Check common pairs (“TH”, “HE”, “IN”) and triples (“THE”, “AND”).  
5. **Iteratively adjust**: Keep testing substitutions until the plaintext makes sense.

---

### Complexity
- **Time**: Manual process but polynomial if automated.  
- **Space**: Frequency table of 26 letters.

---

### Weakness (for Substitution Ciphers)
- Substitution is **not random** → ciphertext leaks statistical patterns.  
- Even with a 26! keyspace, substitution is insecure because **patterns reveal mappings**.

---

### Metaphor
Imagine reading a coded diary where one symbol appears far more than others.  
If you know in English the word “the” appears constantly, you guess that symbol must be “E.”  
It’s like solving a crossword puzzle: the **shapes and patterns** reveal the hidden words.


---

## 4. Summary & Takeaways
- **Caesar cipher**: First step in cryptography, but very weak.  
- **Substitution cipher**: Stronger, but still broken with analysis.  
- **Frequency analysis**: First systematic cryptanalysis tool.  
- These ciphers are no longer secure but serve as **building blocks** for modern cryptography.
