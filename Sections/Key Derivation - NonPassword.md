
[ Key Derivation ]: https://doc.libsodium.org/key_derivation
[ Nonce Extension ]: https://doc.libsodium.org/key_derivation#nonce-extension

[ Blake 2 ]: https://www.blake2.net/blake2.pdf
[ Blake 3 ]: https://github.com/BLAKE3-team/BLAKE3#the-blake3-crate-
[ Blake 3 Specs ]: https://github.com/BLAKE3-team/BLAKE3-specs/blob/master/blake3.pdf
[ Blake 3 Github ]: https://github.com/BLAKE3-team/BLAKE3

[ RFC5869 ]: https://datatracker.ietf.org/doc/html/rfc5869#section-3.1

[ HKDF ]: https://en.wikipedia.org/wiki/HKDF
[ PBKDF2 ]: https://en.wikipedia.org/wiki/PBKDF2
[ XSalsa ]: https://cr.yp.to/snuffle/xsalsa-20110204.pdf
[ Pepper ]: https://en.wikipedia.org/wiki/Pepper_(cryptography)

[ Bruteforce Attack ]: https://en.wikipedia.org/wiki/Brute-force_attack
[ Length Extension Attack ]: https://en.wikipedia.org/wiki/Length_extension_attack





# (Non-Password-Based) Key Derivation Functions

<br>
<br>
<br>

## Use 「 Ordered 」

### [Salted BLAKE2b][ Key Derivation ]
[Restricted][ Blake 2 ] to a **128-bit** salt and **128-bit** (16 character) personalization parameter for domain separation,<br>
which is annoying. However, you can feed more context information into the message parameter.<br><br>
Besides the weird personal size limit, this is easier to use than **HKDF** because there’s only one function rather than three,<br>
which can be confusing. Furthermore, please see the [Hashing](./Hashing) section for why **BLAKE2b** should be preferred over other<br>
hash functions. If there's no KDF variant of **BLAKE2b** available in your library, then you should probably use **HKDF**<br>
(please see below).<br><br>
However, **if you know what you're doing**, you can construct a **BLAKE2b KDF** using<br><br>
`BLAKE2b(message: salt || info || saltLength || infoLength, key: inputKeyingMaterial)`<br><br>
with the `saltLength` and `infoLength` parameters being encoded as specified in point **5**<br>of the [Message Authentication Codes](./Message%20Authentication) Notes section.<br><br>Like **HKDF**, this custom approach allows for salt and info parameters of practically any length.

---

### [HKDF-SHA512][ HKDF ] | [HKDF-SHA3-512][ HKDF ]

The most popular **KDF** with support for a larger salt and lots of context information. However,<br>
people get confused about the difference between the Expand and Extract functions, it doesn’t require<br>a salt despite it being [recommended and beneficial for security][ RFC5869 ], and it’s slower than salted **BLAKE2b**.<br><br>
Please see the [Hashing](./Hashing) and [Message Authentication Codes](./Message%20Authentication) sections<br>
for a comparison between **SHA2** / **SHA3** and **HMAC-SHA2** / **HMAC-SHA3**.

---

### [BLAKE3][ Blake 3 ]

As mentioned before, **BLAKE3** has a [lower security margin][ Blake 3 Specs ], but it also doesn’t have a salt parameter.<br>
With that said, [very good guidance][ Blake 3 ] is given on how to produce globally unique and application specific<br>
context strings in the [official GitHub repo][ Blake 3 Github ]. If you'd like a salt parameter, then you can construct<br>
a custom **KDF** implementation as explained in point 1 above.


<br>
<br>
<br>

## Avoid 「 Unordered | All Unsuitable 」

### Regular (salted or unsalted) hash functions

Whilst this *can* be fine for deriving an encryption key from a Diffie-Hellman shared secret for example,<br>
it’s typically **not recommended**. Just use an actual KDF when possible as there’s less that can go wrong<br>
(e.g. there's no risk of [length extension attacks][ Length Extension Attack ]).

---

### Password-based KDFs ([PBKDF2][ PBKDF2 ])

If you’re not using a password, then you shouldn’t be using a password-based KDF.<br>
Password-based **KDFs** are designed to be slow to prevent [bruteforce attacks][ Bruteforce Attack ], whereas<br>
non-password-based **KDFs** are fast because they're designed for high-entropy keys.<br><br>
Even with a small delay (e.g. 1 iteration of **PBKDF2**), this is likely slower and makes<br>
the code more confusing because an inappropriate function is being used.

---

### [HChaCha20][ Nonce Extension ] | [HSalsa20][ XSalsa ]
**These are not general-purpose cryptographic hash functions**, can only take a **256-bit** key<br>
as input and output a **256-bit** key, and are very rarely used, except in the case of implementing<br>
**XChaCha20** and **XSalsa20**. If you want something based on **ChaCha**, then use **BLAKE2b** or **BLAKE3**.


<br>
<br>
<br>

#### Notes

<br>

1. **These KDFs are not suitable for hashing passwords**<br><br>
They should be used for deriving multiple subkeys<br>
from a **high-entropy** master key or converting a<br>
shared secret into a cryptographically strong secret key.

---

2. Using the same parameters besides changing the output length can result in related outputs<br>(e.g. for **HKDF** and **BLAKE3**): this is exactly why you shouldn’t reuse the same parameters for different keys.

---

3. Use different contexts for different keys<br><br>
A good format is `[application] [date and time] [purpose]` because this means the<br>
context information is application-specific and unique, which provides domain separation.

---

4. Salted BLAKE2b can use a **counter** salt<br><br>
If you’re deriving multiple subkeys from a master key, then you can use a counter salt<br>
starting at 0 (16 bytes of 0s) that gets incremented for each subkey. However, if you’re<br>
deriving a single key (e.g. from a shared secret), then you should use a random salt.

---

5. Use a **random** salt with **HKDF** when possible<br><br>
The [RFC][ RFC5869 ] explains that using a random salt adds to the security of the scheme.<br>
The authors recommend a salt as long as the hash output length, but a **128-bit** or **256-bit**<br>
salt is sufficient. Using a secret salt (a bit like a [pepper][ Pepper ] further improves the security guarantees.
