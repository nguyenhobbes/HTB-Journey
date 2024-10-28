### The Last Dance: 
Cipher ChaCha20, reused nonce. If two ciphertexts were generated using the same key and nonce, an attacker can XOR the two ciphertexts together to cancel out the key stream, revealing the XOR of the two plaintexts. Calculate C1 XOR C2 = M1 XOR M2. We have M1, then solve for M2.

### 1. Baby Time Capsule: 
RSA, small e (5). Use CRT on multiple cipher text and public key
