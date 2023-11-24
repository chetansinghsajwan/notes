It is the technique of decrypting encrypted messages.
Cryptanalysis experts study ciphers, cryptosystems, and ciphertext to understand their functions. Then, they use that knowledge to find or improve techniques to weaken or defeat them.
So, a cryptographer is someone who writes encryption code used in cybersecurity, while a cryptoanalyst is someone who tries to crack those encryption codes.
The person practicing Cryptanalysis is called a **Cryptanalyst**.
Types of Cryptanalysis attacks:
- **Known-Plaintext Analysis (KPA)**
    
    In this type of attack, some plaintext-ciphertext pairs are already known. Attacker maps them in order to find the encryption key. This attack is easier to use as a lot of information is already available.
    
- **Chosen-Plaintext Analysis (CPA)**
    
    In this type of attack, the attacker chooses random plaintexts and obtains the corresponding ciphertexts and tries to find the encryption key.
    
- **Ciphertext-Only Analysis (COA)**
    
    In this type of attack, only some cipher-text is known and the attacker tries to find the corresponding encryption key and plaintext. Its the hardest to implement but is the most probable attack as only ciphertext is required.
    
- **Man-In-The-Middle (MITM) attack**
    
    In this type of attack, attacker intercepts the message/key between two communicating parties through a secured channel.
    
- **Adaptive Chosen-Plaintext Analysis (ACPA)**
    
    This attack is similar CPA. Here, the attacker requests the cipher texts of additional plaintexts after they have ciphertexts for some texts.
    
- **Birthday attack**
    
    This attack exploits the probability of two or more individuals sharing the same birthday in a group of people. In cryptography, this attack is used to find collisions in a hash function.
    
- **Brute-force attack**
    
    This attack involves trying every possible key until the correct one is found. While this attack is simple to implement, it can be time-consuming and computationally expensive, especially for longer keys.