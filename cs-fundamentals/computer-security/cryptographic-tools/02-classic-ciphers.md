# 02. Classic Ciphers

## Section Plan
1. Caesar Cipher
2. Substitution Cipher
3. Frequency Analysis
4. Summary & Takeaways

---

## 1. Caesar Cipher
- **Definition**: A shift cipher where each letter is shifted by a fixed number of positions in the alphabet.  
- **Example**: With a shift of 3 → A → D, B → E, C → F.  
- **Formula**:  
  Encryption: `C = (P + k) mod 26`  
  Decryption: `P = (C - k) mod 26`  
  where `P` = plaintext letter, `C` = ciphertext letter, `k` = key (shift amount).  

- **Weakness**: Only 25 possible shifts → trivial brute-force attack.  
- **Metaphor**: Like moving all the books in a library 3 shelves over. Anyone who knows the rule can undo it quickly.

---

## 2. Substitution Cipher
- **Definition**: Each letter of the alphabet is replaced with another letter/symbol according to a fixed mapping.  
- **Example**: A → Q, B → Z, C → K, … (mapping must cover entire alphabet).  
- **Strength vs Caesar**: More possible keys (26! ≈ 4 × 10^26).  
- **Weakness**: Patterns in natural language still leak through.  

---

## 3. Frequency Analysis
- **Definition**: A method to break substitution ciphers by studying how often certain letters or combinations appear.  
- **English Example**:  
  - ‘E’ is the most common letter.  
  - Common digrams (two letters): “TH”, “HE”.  
  - Common trigrams (three letters): “THE”, “AND”.  

- **How it Works**:  
  If the ciphertext has one symbol that appears most often, it probably represents “E” or “T”.  

- **Metaphor**: Like solving a crossword puzzle by noticing which letters show up the most often in English.

---

## 4. Summary & Takeaways
- **Caesar cipher**: First step in cryptography, but very weak.  
- **Substitution cipher**: Stronger, but still broken with analysis.  
- **Frequency analysis**: First systematic cryptanalysis tool.  
- These ciphers are no longer secure but serve as **building blocks** for modern cryptography.
