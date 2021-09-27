
[ Curve25519 ]: https://en.wikipedia.org/wiki/Curve25519
[ Curve448 ]: https://en.wikipedia.org/wiki/Curve448
[ Pre Shared Keys ]: https://en.wikipedia.org/wiki/Pre-shared_key
[ Post Quantum Security ]: https://media.defense.gov/2021/Aug/04/2002821837/-1/-1/1/Quantum_FAQs_20210804.PDF
[ Asymmetric Key Exchange ]: https://www.wireguard.com/protocol/#key-exchange-and-data-packets
[ Plain RSA ]: https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Attacks_against_plain_RSA
[ RSA PKCS ]: https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Padding_schemes
[ RSA KEM ]: https://en.wikipedia.org/wiki/Key_encapsulation
[ RSA OAEP ]: https://en.wikipedia.org/wiki/Optimal_asymmetric_encryption_padding
[ Protecting RSA ]: https://paragonie.com/blog/2018/04/protecting-rsa-based-protocols-against-adaptive-chosen-ciphertext-attack
[ ElGamal ]: https://en.wikipedia.org/wiki/ElGamal_encryption
[ Malleability ]: https://en.wikipedia.org/wiki/Malleability_%28cryptography)
[ NIST Curves ]: https://en.wikipedia.org/wiki/Elliptic-curve_cryptography#Fast_reduction_(NIST_curves)
[ RFC8031 ]: https://datatracker.ietf.org/doc/html/rfc8031#section-4
[ Looks Bad ]: https://safecurves.cr.yp.to/rigid.html
[ NSA Backdoor ]: https://en.wikipedia.org/wiki/Dual_EC_DRBG#Weakness:_a_potential_backdoor
[ Dual EC DRBG ]: https://en.wikipedia.org/wiki/Dual_EC_DRBG
[ Point Validation ]: https://crypto.stackexchange.com/questions/51320/ecdh-check-points
[ Difficult Implementation ]: https://safecurves.cr.yp.to/
[ TLS 1.3 ]: https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.3)
[ Curve41417 ]: https://eprint.iacr.org/2014/526.pdf
[ Usage ]: https://en.wikipedia.org/wiki/Comparison_of_TLS_implementations#Supported_elliptic_curves
[ SafeCurves ]: https://safecurves.cr.yp.to/index.html
[ SRP ]: https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol
[ J-PAKE ]: https://en.wikipedia.org/wiki/Password_Authenticated_Key_Exchange_by_Juggling
[ PAKE ]: https://en.wikipedia.org/wiki/Password-authenticated_key_agreement
[ Odd Design ]: https://blog.cryptographyengineering.com/should-you-use-srp/
[ ECDH ]: https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman
[ Pre Computation Attack ]: https://blog.cryptographyengineering.com/2018/10/19/lets-talk-about-pake/
[ OPAQUE ]: https://blog.cryptographyengineering.com/2018/10/19/lets-talk-about-pake/
[ Adoption ]: https://blog.cloudflare.com/opaque-oblivious-passwords/
[ Post Quantum Algorithms ]: https://csrc.nist.gov/projects/post-quantum-cryptography
[ Key Exchange ]: https://doc.libsodium.org/key_exchange#notes
[ Elligator 2 ]: https://elligator.cr.yp.to/
[ PURB ]: https://bford.info/pub/sec/purb.pdf
[ Patterns ]: http://noiseprotocol.org/noise.html#one-way-handshake-patterns
[ Public Key ]: https://neilmadden.blog/2018/11/26/public-key-authenticated-encryption-and-why-you-want-it-part-ii/
[ Handshakes ]: https://noiseprotocol.org/noise.html#interactive-handshake-patterns-fundamental
[ Hardware Security Modules ]: https://en.wikipedia.org/wiki/Hardware_security_module
[ AWS KMS ]: https://aws.amazon.com/kms/
[ Cryptoperiod ]: https://en.wikipedia.org/wiki/Cryptoperiod

[ Overview ]: ../Overview


# Key Exchange/Hybrid Encryption
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

### [Curve25519 / X25519][ Curve25519 ]

- Fixes some **NIST** curves issues
- Not designed by **NIST**
- `~128-bit security`
- Easy to implement
- Popular
- Fast

---

### [Curve448 / X448][ Curve448 ]

- Also not made by **NIST**
- Slower than **X25519**
- `224-bit security`
- Less popular

---

### [Pre-shared Symmetric Keys][ Pre Shared Keys ]

This approach allows for [post-quantum security][ Post Quantum Security ] and can<br>
be combined [alongside][ Asymmetric Key Exchange ] an asymmetric key exchange.

However, using pre-shared keys can be difficult since the key must be kept secret,<br>
whereas public keys are meant to be public and can therefore be easily shared.


<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

<br>

### RSA ( [Plain][ Plain RSA ] | [PKCS#1 v1.5][ RSA PKCS ] | [KEM][ RSA KEM ] | [OAEP][ RSA OAEP ] )

- Plain / textbook **RSA** is **insecure** for [several reasons][ Plain RSA ]
- **RSA PKCS#1 v1.5** is also **vulnerable** to some [attacks][ RSA PKCS ]
- **RSA-KEM** and **RSA-OAEP**, whilst both secure *when* [implemented correctly][ Protecting RSA ]),<br> are still **worse than using hybrid encryption** because asymmetric encryption is slower,<br> designed for small messages, doesn’t provide sender authentication without signatures,<br>and requires larger keys.
- **RSA-KEM** is also never used and very rarely available in cryptographic libraries.

---

### [ ElGamal ]

- It doesn’t provide sender authentication without signatures
- Produces a ciphertext that’s larger than the plaintext
- Can only be used on small messages
- The design is [malleable][ Malleability ]
- It's slower than hybrid encryption
- Very rarely used
- Old

---

### [ NIST Curves ] ( **P-256** | **P-384** | **P512** )

Although **P-256** is probably the most popular curve, the seeds for these curves<br>
[haven’t been explained][ RFC8031 ], which is [not a good look][ Looks Bad ] considering that [ Dual EC DRBG ]<br>
was a **NIST** standard despite containing an [ NSA Backdoor ].

<br>

Furthermore, these curves:
- Require [ Point Validation ]
- Are [harder to write implementations for][ Difficult Implementation ], meaning<br>
  libraries are more likely to contain vulnerabilities
- And are slower than **Curve25519** / **X25519**, which has become<br>
  increasingly popular over recent years (it's used in [ TLS 1.3 ])

<br>

**These should only be used for interoperability reasons**.

---

### Other Curves
**Such as [ Curve41417 ]**

These are often [rarely used/available][ Usage ] compared<br>
to **Curve25519** / **X25519**, **P-256**, **P-384**, and **P-512**.

Please see the [ SafeCurves ] tables for a security comparison of most curves.

---

### 〔 [ SRP ] 〕 〔 [ J-PAKE ] 〕 〔 [ PAKE ] 〕

Note that these are only for ***password-based authenticated key exchange***.

**SRP** has an [ Odd Design ], no security proof, leaks the salt to untrusted users, requires<br> more bandwidth and computation than [ ECDH ], and old versions contained vulnerabilities.

Some **PAKEs** require the user to store the password on the server or<br>
reveal the salt to an attacker, allowing for [ Pre Computation Attack ].

Furthermore, very few cryptographic libraries include **PAKEs**, which makes good<br>
ones, like [ OPAQUE ], difficult to recommend [until they receive more adoption][ Adoption ].

---

### [ Post Quantum Algorithms ]

- Aren’t implemented in mainstream libraries
- Are much slower than existing algorithms
- Typically have very large key sizes
- Still in research

However, it will eventually make sense to switch to one in the future.

For now, if post-quantum security is a goal, then use a pre-shared symmetric key if possible.

<br>
<br>
<br>

## **Notes**

<br>

「 **1** 」

**Handling of different keys**

**Public**: *Should be* ***shared***<br>
**Private**: *Must be* ***kept secret***


↳ `Check point 9 for details about secure storage of private keys`

---

「 **2** 」

**Never hard-code private keys into source code**

These can be easily retrieved.

---

「 **3** 」

**Hybrid Encryption Algorithms**

Make use of the recommended [Symmetric Encryption Algorithms](./Symmetric%20Encryption)<br>
when using one of the recommended **Hybrid Encryption** algorithms

For example: `X25519` + `ChaCha20-Poly1305`

---

「 **4** 」

**Shared Secret KDFs**

Use one of the recommended [Non-Password Based KDFs](./Key%20Derivation%20-%20NonPassword) on the shared secret.

**Shared secrets are not suitable for use as secret keys directly**<br>
*because they aren't uniformly random.*


Moreover, you should derive unique keys each time by using the salt<br>
and context parameters, as explained in [Key Derivation Functions](./Key%20Derivation%20-%20NonPassword).

---

「 **5** 」

When using counter nonces for encryption, use different<br>
keys for different directions in a `Client ⬌ Server` scenario

After computing the shared secret,<br>
you can use a non-password based **KDF**<br>
to derive two **256-bit** keys as follows:

```
HKDF-SHA512 (
    Info : Client Public Key | Server Public Key
    Input Keying Material : Shared Secret
    Output Length : 64
    Salt : null
)
```

Splitting the output in two.

One key should be used by the client for sending data to the server, and<br>
the other should be used by the server for sending data to the client.

Both keys need to be calculated by the client and server.

This approach allows counter nonces to be used [safely][ Key Exchange ] for encryption<br>
without having to wait for an acknowledgment after every message.

---

「 **6** 」

**X25519** and **X448** public keys can be distinguished from random data

If you need to obfuscate public keys so that they’re indistinguishable<br>
from random noise, then you need to use [ Elligator 2 ].

Note that other metadata (the number of bytes in a packet)<br>
can reveal the use of cryptography too, so you should consider<br>
padding such information using a scheme like [PADME][ PURB ].

---

「 **7** 」

Use an **authenticated key exchange** in most non-interactive / offline protocols

The **Noise** protocol framework K and X one-way handshake [ Patterns ],<br>
as explained [here][ Public Key ], are perfect for non-interactive / offline protocols.

These achieve sender and recipient authentication whilst preventing a compromise<br>
of the sender’s private key leading to an attacker being able to decrypt the ciphertext.

---

「 **8** 」

Opt for **forward secrecy** when possible in interactive / online protocols

This prevents a compromise of a long-term private key leading to a compromise<br>
of a session key, which is the strongest security guarantee you can achieve.

This can be implemented using the Noise KK or IK interactive [handshakes][ Handshakes ].

---

「 **9** 」

**Store Private Keys Encrypted**

When storing a private key in a file, you should always<br>
encrypt it with a strong password for protection at rest.

Things become more complicated for interactive/online scenarios,<br>
with physical or virtual [hardware security modules (HSMs)][ Hardware Security Modules ] and key<br>
vaults, such as [AWS Key Management Service (KMS)][ AWS KMS ],<br>
sometimes being used.

These types of solutions are generally regarded as more secure than storing keys in encrypted configuration files and allow for easy key rotation, but using a **KMS** requires trusting a third party.

---

「 **10** 」

**Key Pairs Should Be Rotated**

If a private key has or may have been compromised, then a new key pair should be generated.

Similarly, you should consider rotating your keys<br>
after a set period of time (a [cryptoperiod][ Cryptoperiod ]) has elapsed.

---

<br>
<br>

##### 「[ Overview ]」
