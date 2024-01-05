# Unit 2

## Confusion and Diffusion

In cryptography, confusion and diffusion are two properties of the operation of a secure cipher.

The confusion technique assures that the ciphertext has no information about the plaintext. The confusion technique keeps the relationship between the **_encrypted text's statistics_** and the **_encryption key's value_** as complex as possible.

In diffusion, the output bits must be highly dependent on the input bits so that if the plaintext is modified by only one bit, the ciphertext must change in an **_unanticipated_** or **_unreliable_** way.

|S.NO|Confusion|Diffusion|
|---|---|---|
|1.|Confusion is a cryptographic technique that is used to create faint cipher texts.|Diffusion is used to create cryptic plain texts.|
|2.|Confusion is possible through substitution algorithms.|Diffusion is possible through transposition algorithms.|
|3.|In confusion, if one bit within the secret is modified, most or all bits within the cipher text also will be modified.|In diffusion, if one image within the plain text is modified, many or all image within the cipher text also will be modified|
|4.|In confusion, vagueness is increased in resultant.|In diffusion, redundancy is increased in the resultant.|
|5.|Both stream cipher and block cipher use confusion. |Only block cipher use diffusion.|
|6.|The relation between the cipher text and the key is masked by confusion.|The relation between the cipher text and the plain text is masked by diffusion.|

## PRNG

PRNG stands for Pseudo Random Number Generator.

It refers to an algorithm that uses mathematical formulas to produce sequences of random numbers.

A PRNG starts from an arbitrary starting state using a seed state.

### Linear Congruence Method

```
Xn+1 = (aXn + c) mod m
```

where `X` is the sequence of pseudo-random values

```
m: modulus (0 < m)
a: multiplier (0 < a < m)
c: increment (0 ? c < m)
x0: seed value (0 ? x0 < m)
```

### Blum Blum Slub Method

```
Xn+1 = (Xn)^2 (mod M)
```

where `x0` is a random seed. The value of `M` is equal to `pq`, and where `p` and `q` are prime numbers. These values of `p` and `q` are both congruent to 3 mod 4.