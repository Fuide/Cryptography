
[ Ed25519 ]: https://en.wikipedia.org/wiki/EdDSA
[ Ed448 ]: https://en.wikipedia.org/wiki/EdDSA#Ed448
[ Plain RSA ]: https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Attacks_against_plain_RSA
[ RSA Padding ]: https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Padding_schemes
[ Probabilistic Signature ]: https://en.wikipedia.org/wiki/Probabilistic_signature_scheme
[ RSA Attacks ]: https://crypto.stackexchange.com/questions/20085/which-attacks-are-possible-against-raw-textbook-rsa
[ RFC8017 ]: https://tools.ietf.org/html/rfc8017#section-8
[ Elliptic Curves ]: https://en.wikipedia.org/wiki/Elliptic-curve_cryptography
[ RSA Survey ]: https://crypto.stanford.edu/~dabo/papers/RSA-survey.pdf
[ ElGamal ]: https://en.wikipedia.org/wiki/ElGamal_signature_scheme
[ CS548 ]: https://caislab.kaist.ac.kr/lecture/2010/spring/cs548/basic/B02.pdf
[ Existential Forgery ]: https://en.wikipedia.org/wiki/ElGamal_signature_scheme#Security
[ DSA ]: https://en.wikipedia.org/wiki/Digital_Signature_Algorithm
[ DSA Insecure Key ]: https://buttondown.email/cryptography-dispatches/archive/cryptography-dispatches-dsa-is-past-its-prime/
[ Elliptic Curve Signature ]: https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm
[ Elliptic Curve Security ]: https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm#Security
[ Secure Pseudo Random ]: https://en.wikipedia.org/wiki/Cryptographically-secure_pseudorandom_number_generator
[ RFC6979 ]: https://datatracker.ietf.org/doc/html/rfc6979#section-3
[ RFC8031 ]: https://datatracker.ietf.org/doc/html/rfc8031#section-4
[ NSA Backdoor ]: https://en.wikipedia.org/wiki/Dual_EC_DRBG#Weakness:_a_potential_backdoor
[ Dual DRBG ]: https://en.wikipedia.org/wiki/Dual_EC_DRBG
[ Quantum Cryptography ]: https://csrc.nist.gov/projects/post-quantum-cryptography
[ Bad Look ]: https://safecurves.cr.yp.to/rigid.html
[ Sign Encryption ]: https://theworld.com/~dtd/sign_encrypt/sign_encrypt7.html
[ Recommendation ]: https://github.com/jedisct1/libsodium/issues/632#issuecomment-345272065
[ Resource Constraints ]: https://monocypher.org/manual/advanced/from_eddsa
[ Multi Part Messages ]: https://doc.libsodium.org/public-key_cryptography/public-key_signatures#multi-part-messages
[ Collisions Resistant ]: https://ed25519.cr.yp.to/eddsa-20150704.pdf
[ Fault Attacks ]: https://eprint.iacr.org/2017/1014.pdf
[ Voltage Glitches ]: https://cybermashup.files.wordpress.com/2017/10/practical-fault-attack-against-eddsa_fdtc-2017.pdf
[ Access Required ]: https://eprint.iacr.org/2017/1014.pdf
[ Countermeasures ]: https://crypto.stackexchange.com/questions/50228/can-deterministic-ecdsa-be-protected-against-fault-attacks?rq=1
[ Signing ]: https://eprint.iacr.org/2017/1014.pdf
[ Signal ]: https://signal.org/docs/specifications/xeddsa/#security-considerations
[ Not Guaranteed ]: https://eprint.iacr.org/2017/985.pdf
[ Side Channel Attack ]: https://en.wikipedia.org/wiki/Side-channel_attack


# Digital Signatures

<br>
<br>
<br>

## Use 「 Ordered 」

### [Ed25519][ Ed25519 ]

Very popular, accessible, fast, uses small keys, produces small signatures,<br>
deterministic, and offers `~128-bit security`.

---

### [Ed448][ Ed448 ]
Less popular and slower than **Ed25519** but uses **SHAKE256** (a **SHA3** variant)<br>
instead of **SHA512** for hashing and **Edwards448** instead of **Edwards25519**<br>
for the curve, meaning a `224-bit security` level.

<br>
<br>
<br>

## Avoid 「 Unordered | All Unsuitable 」

### [Plain RSA][ Plain RSA ] | [RSA-PSS][ Probabilistic Signature ] | [RSA-PKCS#1 v1.5][ RSA Padding ]

- **Plain RSA** is [insecure][ RSA Attacks ].

- **RSA-PKCS#1 v1.5** has [no security proof][ Probabilistic Signature ] and is [no longer recommended in the RFC][ RFC8017 ].
- **RSA-PSS** is slow for signing and generating keys, produces larger signatures,<br>
    and requires larger keys than [ECC][ Elliptic Curves ] based signing algorithms.<br>

Moreover, **RSA** has [implementation traps][ RSA Survey ].

---

### [ElGamal][ ElGamal ]

- Old
- Slower than **RSA**
- Not included in cryptographic libraries
- Basically not used in any software
- Not standardized
- Produces large signatures

Allows for [existential forgery][ Existential Forgery ] if the message is used directly rather than the hash,<br>
as specified in the [original paper][ CS548 ].

---

### [DSA][ DSA ]

- Very old
- Slower than **Ed25519**
- Decreasing support
- Typically used with an [insecure key size][ DSA Insecure Key ]
- Requires larger keys than [ECC][ Elliptic Curves ]

DSA is not deterministic, which has led to [serious vulnerabilities][ Elliptic Curve Security ] (please see below).

---

### [ECDSA][ Elliptic Curve Signature ]

Slower than **Ed25519** and not deterministic, which has led to [serious vulnerabilities][ Elliptic Curve Security ]<br>
that affected Sony’s PS3 and Bitcoin, allowing attackers to recover private keys.<br>
<br>
This issue can be prevented by properly generating a random nonce,<br>
which requires having a good [CSPRNG][ Secure Pseudo Random ], or by deriving the nonce deterministically<br>
using [something like HMAC][ RFC6979 ].<br>
<br>
However, there’s been a shift to Ed25519 because it prevents this issue from happening<br>
as well as being better in other respects. Furthermore, there’s also the concern mentioned<br>
in the [Key Exchange/Hybrid Encryption](./Hybrid%20Encryption) Avoid section that the NIST curves use [unexplained seeds][ RFC8031 ],<br>which is [not a good look][ Bad Look ] considering that [Dual_EC_DRBG][ Dual DRBG ] was a NIST standard despite<br>
containing an [NSA backdoor][ NSA Backdoor ].

---

### [Post Quantum Algorithms][ Quantum Cryptography ]

- Still in research
- No implementation in mainstream libraries
- Much Slower
- Large key size

However, eventually it will make sense to switch to one in the future.

<br>
<br>
<br>

## Notes

<br>

1. Please read points **1**, **2**, **9**, and **10** of the [Key Exchange/Hybrid Encryption Notes ](./Hybrid%20Encryption) section<br>
because all these points about key pairs/private keys apply for signature algorithms as well.

---

2. Use authenticated hybrid encryption (an authenticated key exchange with authenticated encryption) <br>
instead of encryption with signatures: this is easier to get right and more efficient.

---

3. Use `Sign ➜ Encrypt` **if you must use signatures with encryption** to provide sender authentication:<br>
`Encrypt ➜ Sign` can allow an attacker to strip off the original signature and replace it with their own.<br><br>
For symmetric encryption, `Sign ➜ Encrypt ➜ MAC`, which involves signing the message,<br>
appending the signature to the message, and using either `Encrypt ➜ MAC` or an `AEAD`,<br>
prevents this problem.<br><br>
Similarly, if you’re forced to use asymmetric encryption, then you can still use Sign-then-Encrypt<br>
but should include the [recipient’s name or the sender and recipient's names in the message][ Sign Encryption ]<br>
because the recipient needs proof that the same person signed and encrypted the message.<br><br>
Once the signature and encryption layers are bound together, an attacker can't remove<br>
and replace the outer layer because the reference in the inner layer will reveal the tampering.<br><br> Alternatively, you can `Encrypt ➜ Sign ➜ Encrypt` or `Sign ➜ Encrypt ➜ Sign`, which are both slower.

---

4. **Don’t** use the same key pair for signatures (e.g. **Ed25519**) and key exchange (e.g. **X25519**):<br>
It’s [recommended][ Recommendation ] to **never use the same key for more than one thing** in cryptography.<br><br>**The security of using the same key pair for these two algorithms has not been (sufficiently) studied**,<br>
signing key pairs and encryption key pairs often have different life cycles, and using different key pairs<br>
limits the damage done if one key pair is compromised.<br><br>
Since the keys are so small, using different key pairs produces barely any overhead as well.<br>
The only time you should really convert an **Ed25519** key pair to an **X25519** key pair is if<br>
you’re [heavily resource constrained][ Resource Constraints ] or when you’re forced to use **Ed25519** keys<br>
(e.g. SSH public keys off GitHub could be used for hybrid encryption).

---

5. Prehash large messages:<br><br>
Signing a message normally requires loading the entire message into memory, but this can be<br>
problematic for very large (e.g. 1+ GiB) messages.<br><br>
To solve this problem, you can use [Ed25519ph][ Multi Part Messages ] or **Ed448ph** (which probably isn't available)<br>
to perform the prehashing for you with some additional domain separation, or you can<br>
prehash the message yourself using a strong, modern hash function, like **BLAKE2b** or **SHA3**,<br>
with a **512-bit** output length and sign the hash instead of the message.<br><br>
However, note that not prehashing means that **Ed25519** is [resistant to collisions in the hash function][ Collisions Resistant ].<br>Therefore, when possible, ordinary signing should arguably be preferred for additional protection,<br>
although this isn't realistically a problem if you use a secure hash function.

---

6. Be aware of [fault attacks][ Fault Attacks ] against deterministic signatures:<br><br>
Techniques like causing [voltage glitches][ Voltage Glitches ] on a chip (e.g. on an Arduino) can be used to recover<br>
either the entire secret key or part of the secret key, depending on the signature algorithm,<br>
and create valid signatures with algorithms like **Ed25519**, **Ed448**, and deterministic **ECDSA**.<br><br>
However, this is primarily a concern on embedded devices and requires [physical or remote access][ Access Required ] to a device.<br><br>
Four [countermeasures][ Countermeasures ] include signing the same data twice and comparing the outputs,<br>
which is obviously slower than signing once, verifying the signature after signing,<br>
which is [slower][ Signing ] than signing twice for small messages but [faster][ Signing ] for large messages,<br>
[calculating a checksum over input values][ Signing ] before and after signature generation,<br>
or using a cryptographic library that implements the algorithm with some random<br>
data in the calculation of the nonce, which is the technique used by [Signal][ Signal ].<br><br>
However, these countermeasures are [not guaranteed to be effective][ Not Guaranteed ]<br>
and there can be other [side-channel attacks][ Side Channel Attack ] as well.
