
[ Generated Hashing ]: https://doc.libsodium.org/hashing/generic_hashing
[ Analysis ]: https://nvlpubs.nist.gov/nistpubs/ir/2012/NIST.IR.7896.pdf
[ Same Security ]: https://eprint.iacr.org/2019/1492.pdf

[ SHA2 Studied ]: https://en.wikipedia.org/wiki/SHA-2#Cryptanalysis_and_validation
[ SHA 3 Slow ]: https://www.imperialviolet.org/2017/05/31/skipsha3.html
[ SHA 3 Security ]: https://csrc.nist.gov/csrc/media/projects/hash-functions/documents/sha-3_selection_announcement.pdf
[ SHA 3 Fast ]: https://keccak.team/2017/is_sha3_slow.html

[ Blake 2 ]: https://www.blake2.net/#us
[ Blake 3 ]: https://github.com/BLAKE3-team/BLAKE3#readme
[ Blake 3 Spec ]: https://github.com/BLAKE3-team/BLAKE3-specs/blob/master/blake3.pdf

[ MAC Hate ]: https://blog.cryptographyengineering.com/2013/02/15/why-i-hate-cbc-mac/
[ CMAC ]: https://en.wikipedia.org/wiki/One-key_MAC
[ KMAC ]: https://en.wikipedia.org/wiki/SHA-3#Additional_instances
[ HMAC Security ]: https://en.wikipedia.org/wiki/HMAC#Security
[ HMAC SHA256 ]: https://doc.libsodium.org/advanced/hmac-sha2
[ HMAC ]: https://en.wikipedia.org/wiki/HMAC
[ HMAC SHA3 256 ]: https://en.wikipedia.org/wiki/SHA-3

[ Length Extension Attacks ]: https://en.wikipedia.org/wiki/Length_extension_attack
[ Poly1305 ]: https://doc.libsodium.org/advanced/poly1305
[ CBC-MAC ]: https://en.wikipedia.org/wiki/CBC-MAC
[ Weird Requirements ]: https://en.wikipedia.org/wiki/CBC-MAC#Security_with_fixed_and_variable-length_messages
[ Keyed Hashes ]: https://doc.libsodium.org/hashing/generic_hashing#usage
[ Canonicalization Attack ]: https://soatok.blog/2021/07/30/canonicalization-attacks-against-macs-and-signatures/

[ RFC9106 ]: https://www.rfc-editor.org/rfc/rfc9106.html#name-introduction

[ Overview ]: ../Overview
[ Symmetric Encryption ]: ./Symmetric%20Encryption
[ Symmetric Key Size ]: ./Symmetric%20Keys


# Message Authentication Codes
##### 「[ Overview ]」


<br>
<br>
<br>


## **Use** 「 In Order 」

<br>

### [ Keyed BLAKE2b ( 256 | 512 ) ][ Generated Hashing ]

- Faster than **HMAC**, **BLAKE** (what **BLAKE2** was on)
- Provides the [same practical level of security][ Same Security ] as **SHA3**
- Popular in software ([Argon2][ RFC9106 ] and many [other][ Blake 2 ] password hashing schemes)
- Received a [significant amount of cryptanalysis][ Analysis ],<br>
  even more than **Keccak** (the **SHA3** finalist),<br>
  as part of the **SHA3** competition

---

### [ HMAC SHA ( 256 | 512 ) ][ HMAC SHA256 ]

Slower and older than **BLAKE2b** but [well-studied][ SHA2 Studied ].

**SHA2** is also faster and far more available than **SHA3**, which has<br>
seen somewhat limited adoption so far since **SHA2** is still secure.

---

### [ HMAC SHA3 ( 256 | 512 ) ][ HMAC SHA3 256 ]

- **SHA3** is [slower][ SHA 3 Slow ] than **BLAKE2b** and **SHA2** in software
- But has a [higher security margin][ SHA 3 Security ] than both
- And is [fast][ SHA 3 Fast ] in hardware.

Moreover, **HMAC-SHA3** is needlessly inefficient because **SHA3** is already a **MAC**.

The only reason I’m recommending **HMAC-SHA3** is because **SHA3** is designed to be<br>
a replacement for **SHA2**, **KMAC** is rarely available, and you shouldn't have to<br>
construct a **MAC** using concatenation yourself in most scenarios.

---

### [Keyed BLAKE3 256][ Blake 3 ]

- [Faster][ Blake 3 Spec ] than **HMAC**, **BLAKE2b**, and **SHA3**
- But it has a [smaller security margin][ Blake 3 Spec ]
- Only targets the [`128-bit security`][ Blake 3 Spec ] level
- And hasn't been implemented in many cryptographic libraries yet


<br>
<br>
<br>


## **Avoid** 「 Unordered | All Unsuitable 」

<br>

### HMAC ( [MD5][ HMAC Security ] | [SHA1][ HMAC ] )

**MD5** and **SHA1** should no longer be used for anything.

---

### (Regular) Unencrypted Hashes

`SHA256( ciphertext )`

This is **insecure** because unkeyed hashes don't provide authentication.

---

### (Regular) Encrypted Hashes

`AES-CTR( SHA256( ciphertext ) )`

This is **insecure**.

For example, with a stream cipher, you could flip bits in the ciphertext hash.

---

### `SHA2( Key || Message )`

This is **vulnerable** to [ Length Extension Attacks ], as<br>
discussed in point **3** of the Notes in the [Hashing](./Hashing) section.

Technically speaking, `SHA2(message || key)` works as a **MAC*** if the attacker<br>
doesn’t know the key, but it’s weaker than constructions like **HMAC** because it<br>
requires the hash function to be collision resistant rather than a<br>
pseudorandom function and therefore shouldn’t be used.

Newer hash functions, like **BLAKE2b**, **SHA3**, and **BLAKE3**, are resistant to<br>
length extension attacks and could be used to perform `Hash(key || message)`<br>
safely, but you should still just use a keyed hash function or HMAC instead<br>
to do the work for you.

---

### [Poly1305][ Poly1305 ]
**And Other Polynomial MACs**

These produce small tags that are designed for online protocols and small messages.

They’re also easier to misuse than the recommended algorithms (e.g. **Poly1305** requires<br>a secret, unique, and unpredictable key each time that’s independent from the encryption key).

---

### [CBC-MAC][ CBC-MAC ]

This is unpopular and often [implemented incorrectly][ MAC Hate ] because it has [weird requirements][ Weird Requirements ]<br>that most people are completely unaware of, **allowing for attacks**.

Even when implemented correctly, the recommended algorithms are better.

---

### [CMAC/OMAC][ CMAC ]

Almost nobody uses this, even though it improves<br>
on **CBC-MAC** in terms of preventing mistakes.

---

### 128-bit ( [ Keyed Hashes ] | [ HMACs ][ HMAC ] )

**You shouldn’t go below a 256-bit output** with hash functions<br>
because `128-bit security` should be the minimum.

---

### [KMAC][ KMAC ]

Whilst more efficient than **HMAC-SHA3**, it seems to be rarely available.

Furthermore, it’s likely that **HMAC-SHA3** will be the norm because<br>
**SHA3** is designed to replace **SHA2**, which is used with **HMAC**.


<br>
<br>
<br>


## **Notes**

<br>

1. **Please read** points **14** - **17** of the [ Symmetric Encryption ]<br>
    notes for guidance on implementing a **MAC** correctly.

---

2. **Please read** point **2** of the [ Symmetric Key Size ]<br>
    Use section for guidance on what key size to use.

---

3. A **256-bit** authentication tag is sufficient for most use cases<br><br>
    However, a **512-bit** tag provides additional security<br>
    if you’re concerned about quantum computing.<br><br>
    I wouldn’t recommend bothering with an output length in-between<br>
    (**HMAC-SHA384**) because that’s not common, and you may as well<br>
    go all the way to get a `256-bit security` level.

---

4. Append the authentication tag to the ciphertext:<br><br>
    This is common practice and how **AEADs** operate.

---

5. Concatenating multiple variable length parameters can lead to **attacks**<br><br>
`HMAC( Message : AdditionalData || Ciphertext , Key : MacKey )`<br><br>
If you fail to concatenate the lengths of the parameters<br><br>
`HMAC( Message : AdditionalData || Ciphertext || AdditionalDataLength || CiphertextLength , Key : MacKey )`<br><br>
with the lengths converted to a fixed number of bytes consistently in either<br>
big- or little-endian, regardless of the endianness of the machine) or always<br>
ensure that they are fixed in size, then your implementation will be susceptible<br>
to [canonicalization attacks][ Canonicalization Attack ] because an attacker can shift bytes in the different<br>
parameters whilst producing a valid authentication tag.<br>
<br>**AEADs** do this length concatenation for you to prevent this.
