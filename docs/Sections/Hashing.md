
[ General Hashing ]: https://doc.libsodium.org/hashing/generic_hashing
[ IRL Secure ]: https://eprint.iacr.org/2019/1492.pdf
[ Significant Analysis ]: https://nvlpubs.nist.gov/nistpubs/ir/2012/NIST.IR.7896.pdf
[ Introduction ]: https://www.rfc-editor.org/rfc/rfc9106.html#name-introduction
[ Blake2 ]: https://www.blake2.net/#us
[ SHA Comparison ]: https://en.wikipedia.org/wiki/SHA-2#Comparison_of_SHA_functions
[ Length Extension Attacks ]: https://en.wikipedia.org/wiki/Length_extension_attack
[ SHA 3 Comparison ]: https://en.wikipedia.org/wiki/SHA-3#Comparison_of_SHA_functions
[ Slow SHA3 ]: https://www.imperialviolet.org/2017/05/31/skipsha3.html
[ New Standard ]: https://www.nist.gov/publications/sha-3-standard-permutation-based-hash-and-extendable-output-functions
[ Fast SHA3 ]: https://keccak.team/2017/is_sha3_slow.html
[ Security Margin ]: https://eprint.iacr.org/2012/421.pdf
[ Blake3 ]: https://github.com/BLAKE3-team/BLAKE3#readme
[ Blake3 Implementation ]: https://github.com/BLAKE3-team/BLAKE3/issues/31
[ Blake3 Specs ]: https://github.com/BLAKE3-team/BLAKE3-specs/blob/master/blake3.pdf
[ SHA3 Candidates ]: https://eprint.iacr.org/2009/378.pdf
[ Cycling Redundancy ]: https://en.wikipedia.org/wiki/Cyclic_redundancy_check
[ MD5 ]: https://en.wikipedia.org/wiki/MD5
[ SHA1 ]: https://en.wikipedia.org/wiki/SHA-1
[ MD5 Collision ]: https://eprint.iacr.org/2013/170.pdf
[ RIPEMD ]: https://en.wikipedia.org/wiki/RIPEMD
[ Whirlpool ]: https://en.wikipedia.org/wiki/Whirlpool_(hash_function)
[ Streetbog ]: https://en.wikipedia.org/wiki/Streebog
[ MD6 ]: https://en.wikipedia.org/wiki/MD6
[ Benchmark ]: https://www.cryptopp.com/benchmarks.html
[ S-Box Design ]: https://eprint.iacr.org/2016/071.pdf
[ Password Cracking ]: https://en.wikipedia.org/wiki/Password_cracking
[ Rainbow Tables ]: https://en.wikipedia.org/wiki/Rainbow_table
[ MAC ]: https://en.wikipedia.org/wiki/Message_authentication_code
[ MAC Security ]: https://en.wikipedia.org/wiki/Message_authentication_code#Security
[ SHA3 ]: https://competitions.cr.yp.to/sha3.html

[ Overview ]: ../Overview


# Hashing
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

<br>

### [ BLAKE2b ( 512 | 256 ) ][ General Hashing ]

- As [real-world secure][ IRL Secure ] as SHA3
- Modern
- Fast

**BLAKE** (what **BLAKE2** was on) received a [significant amount of cryptanalysis][ Significant Analysis ],<br>
even more than **Keccak** (the **SHA3** finalist), as part of the **SHA3** competition,<br>
and now quite popular in software (e.g. it’s used in [Argon2][ Introduction ] and many [other][ Blake2 ]<br>
    password hashing schemes).

---

### [ SHA ( 256 | 512 ) ][ SHA Comparison ]

**SHA2** is the most popular hash function, meaning it’s widely available in<br>
cryptographic libraries, it’s still secure besides [ Length Extension Attacks ]<br>
(please see point **3** of the Notes section) and it offers decent performance.

---

### [ SHA3 ( 256 | 512 ) ][ SHA 3 Comparison  ]

- [Slow][ Slow SHA3 ] in software
- But the [new standard][ New Standard ]
- [Fast][ Fast SHA3 ] in hardware
- Has a [higher security margin][ Security Margin ] than the other algorithms listed here.

If it was more common in software, then I would recommend it over **SHA2**<br>
to prevent length extension attacks, but I’d still recommend **BLAKE2b** in such<br>
a case due to the improved software performance and equivalent security.

---

### [BLAKE3 256][ Blake3 ]

The [fastest][ Blake3 Specs ] cryptographic hash in software (assuming you don’t suffer from<br>
[performance issues in the main implementation][ Blake3 Implementation ]) at the cost of having a<br>
[lower security margin][ Blake3 Specs ] and being limited to a [`128-bit security`][ Blake3 Specs ] level.

However, it improves on **BLAKE2** in that there’s only one variant that covers<br>
all use cases (it’s a regular **hash**, **PRF**, **MAC**, **KDF**, and **XOF**), but depending on<br>
the cryptographic library you use, this isn’t necessarily something you’ll notice<br>
when using **BLAKE2b** anyway.


<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

<br>

### **Non-cryptographic** Hash Functions
**And Error-Detecting Codes**

Such as [CRC][ Cycling Redundancy ]. The clue is in the name, these are **not secure**.

---

### 〔 [ MD5 ] 〕〔 [ SHA1 ] 〕

Both are very old and **no longer secure**.

For instance, there’s an [attack][ MD5 Collision ] that breaks **MD5** collision resistance in `2 ^ 18` time.<br>
This takes less than a second to execute on an ordinary computer.

---

### **Insecure** SHA3 Candidates

Such as [EDON-R][ SHA3 Candidates ]

If you want to use something from the **SHA3** competition, then consider:

**BLAKE2b**:
- Based on **BLAKE**
- Thoroughly analyzed
- Deemed to have a [very high security margin][ Significant Analysis ]

**SHA3**:
- Winner of the competition
- Very different in design to **SHA2**
- Has a [very high security margin][ Significant Analysis ]

**BLAKE3**:
- Based on **BLAKE2**
- But with a [lower security margin][ Blake3 Specs ]

---

### [RIPEMD][ RIPEMD ]

- Most implementations are limited to<br>
  small output lengths (commonly **160-bit**)
- Hasn't received a lot of analysis
- Worse performance
- Unpopular
- Old

---

### 〔 [ Whirlpool ] 〕  〔 [SHA224][ SHA Comparison ] 〕  〔 [ Streetbog ] 〕  〔 [ MD6 ] 〕
**And Other Uncommonly Used Hashes**

*These are all worse in one way or another than the<br>*
*recommended algorithms, which is why they aren't used.*

<br>

For instance:
- **Whirlpool** is [slower][ Benchmark ] than most other cryptographic hash functions
- **Streetbog** has a [poor S-Box design][ S-Box Design ] with no design rational ever being made public
- **MD6** didn't make it to the [second round][ SHA3 ] of the **SHA3** competition and has [speed issues][ MD6 ]
- **SHA224** only provides [112-bit collision resistance][ SHA Comparison ],<br>
  which is below the recommended `128-bit security` level

---

### Chained Hashes

`HASH-B( HASH-A( Message ) )`

This can be **insecure**.

<br>

**SHA1** for example has worse collision resistance than **SHA256**,<br>
meaning a collision for **SHA1** results in a collision for

`SHA256( SHA1( Message ) )`

and is obviously less efficient than hashing once.

<br>

**Just don’t do this**.

---

### 128-Bit Hashes

You shouldn’t go below a **256-bit** output with<br>
hash functions to ensure `128-bit security`.


<br>
<br>
<br>

## **Notes**

<br>

「 **1** 」

**These hash functions are not suitable for password hashing.**

These algorithms are fast, whereas password hashing<br>
needs to be slow to prevent [bruteforce attacks][ Password Cracking ].

Furthermore, password hashing requires using a **random** salt<br>
for each password to derive unique hashes when given the same<br>
input and to protect against attacks using [precomputed hashes][ Rainbow Tables ].

---

「 **2** 」

**These unkeyed hash functions are not suitable for authentication.**

You need to use [MACs][ MAC ] such as keyed **BLAKE2b-512** and **HMAC-SHA512**<br>
for authentication because they provide the appropriate [security guarantees][ MAC Security ].<br>
↳ [ `Check out the Message Authentication Codes section` ](./Message%20Authentication)


---

「 **3** 」

**Beware Of Algorithms Susceptible To [ Length Extension Attacks ]**

<br>

Such as:

- **SHA2** (except for **SHA512/256**)<br>
  **SHA224** and **SHA384** don’t provide the same [level of protection][ SHA Comparison ]
- **RIPEMD-160**
- **Whirlpool**
- **SHA1**
- **MD4**
- **MD5**

<br>

An attacker can use `Hash( MessageA )` and the length of `MessageA`<br>
to calculate `Hash( MessageA | MessageB )`, with `MessageB` being<br>
controlled by the attacker, without knowing what `MessageA` is.

Therefore, ***concatenating things*** like `Hash( Secret | Message )`<br>
with these algorithms ***is a bad idea***.

Instead consider:
- **BLAKE2b**
- **SHA512/256**
- **HMAC-SHA2**
- **SHA3**
- **HMAC-SHA3**
- **BLAKE3**

as none of these are susceptible to [ Length Extension Attacks ].

Also, please read point **5** of the [Message Authentication Codes](./Message%20Authentication) notes as<br>
concatenating parameters incorrectly ***can lead to another type of attack***.
