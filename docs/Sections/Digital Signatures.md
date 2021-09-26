
[ Ed25519 ]: https://en.wikipedia.org/wiki/EdDSA
[ Ed448 ]: https://en.wikipedia.org/wiki/EdDSA#Ed448


[ RSA Padding ]: https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Padding_schemes
[ RSA Attacks ]: https://crypto.stackexchange.com/questions/20085/which-attacks-are-possible-against-raw-textbook-rsa
[ RSA Survey ]: https://crypto.stanford.edu/~dabo/papers/RSA-survey.pdf
[ Plain RSA ]: https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Attacks_against_plain_RSA

[ ElGamal ]: https://en.wikipedia.org/wiki/ElGamal_signature_scheme

[ CS548 ]: https://caislab.kaist.ac.kr/lecture/2010/spring/cs548/basic/B02.pdf
[ Probabilistic Signature ]: https://en.wikipedia.org/wiki/Probabilistic_signature_scheme
[ Existential Forgery ]: https://en.wikipedia.org/wiki/ElGamal_signature_scheme#Security
[ Secure Pseudo Random ]: https://en.wikipedia.org/wiki/Cryptographically-secure_pseudorandom_number_generator

[ DSA ]: https://en.wikipedia.org/wiki/Digital_Signature_Algorithm
[ DSA Insecure Key ]: https://buttondown.email/cryptography-dispatches/archive/cryptography-dispatches-dsa-is-past-its-prime/

[ Elliptic Curves ]: https://en.wikipedia.org/wiki/Elliptic-curve_cryptography
[ Elliptic Curve Signature ]: https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm
[ Elliptic Curve Security ]: https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm#Security

[ RFC8017 ]: https://tools.ietf.org/html/rfc8017#section-8
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

[ Overview ]: ../Overview


# Digital Signatures
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

### [ Ed25519 ]

- Offers `~128-bit security`
- Produces small signatures
- Uses small keys
- Deterministic
- Very popular
- Accessible
- Fast

---

### [ Ed448 ]

- Less popular and slower than **Ed25519** but uses **SHAKE256** (a **SHA3** variant)<br>
instead of **SHA512** for hashing and **Edwards448** instead of **Edwards25519**<br>
for the curve, meaning a `224-bit security` level.

<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

<br>

### RSA ( [Plain][ Plain RSA ] | [PSS][ Probabilistic Signature ] | [PKCS#1 v1.5][ RSA Padding ] )

- **RSA-PKCS#1 v1.5** has [no security proof][ Probabilistic Signature ] and is [no longer recommended in the RFC][ RFC8017 ].
- **Plain RSA** is [insecure][ RSA Attacks ].
- **RSA-PSS**:
    - Is slow for signing and generating keys
    - Produces larger signatures
    - Requires larger keys than [ECC][ Elliptic Curves ] based signing algorithms

Moreover, **RSA** has [implementation traps][ RSA Survey ].

---

### [ElGamal][ ElGamal ]

- Not included in cryptographic libraries
- Basically not used in any software
- Produces large signatures
- Not standardized
- Slower than **RSA**
- Old

Allows for [existential forgery][ Existential Forgery ] if the message is used directly<br>
rather than the hash, as specified in the [original paper][ CS548 ].

---

### [DSA][ DSA ]

- Typically used with an [insecure key size][ DSA Insecure Key ]
- Requires larger keys than [ECC][ Elliptic Curves ]
- Slower than **Ed25519**
- Decreasing support
- Very old

**DSA** is not deterministic, which has led to [serious vulnerabilities][ Elliptic Curve Security ].<br>
↳ `Please see below`

---

### [ECDSA][ Elliptic Curve Signature ]

- Slower than **Ed25519**
- Not deterministic, which has led to [serious vulnerabilities][ Elliptic Curve Security ] that affected<br>
  Sony’s PS3 and Bitcoin, ***allowing attackers to recover private keys***.

This issue can be prevented by properly generating a random nonce,<br>
which requires having a good [CSPRNG][ Secure Pseudo Random ], or by deriving the nonce<br>
deterministically using [something like HMAC][ RFC6979 ].<br>

However, there’s been a shift to **Ed25519** because it prevents this<br>
issue from happening as well as being better in other respects.

Furthermore, there’s also the concern mentioned in [Hybrid Encryption](./Hybrid%20Encryption)<br>
that the **NIST** curves use [unexplained seeds][ RFC8031 ], which is [not a good look][ Bad Look ]<br>
considering that [Dual_EC_DRBG][ Dual DRBG ] was a **NIST** standard despite containing<br>
an [NSA backdoor][ NSA Backdoor ].

---

### [Post Quantum Algorithms][ Quantum Cryptography ]

- No implementation in mainstream libraries
- Still in research
- Large key size
- Much Slower

However, eventually it will make sense to switch to one in the future.

<br>
<br>
<br>

## **Notes**

<br>

「 **1** 」

Please read points **1** - **2** and **9** - **10** of the [Hybrid Encryption](./Hybrid%20Encryption) notes because all<br>
these points about key pairs / private keys apply for signature algorithms as well.

---

「 **2** 」

Use authenticated hybrid encryption instead of encryption with signatures.<br>
(Authenticated key exchange with authenticated encryption)

This is easier to get right and more efficient.

---

「 **3** 」

**If you must use signatures with encryption**:<br>
Use `Sign ➜ Encrypt` to provide sender authentication


`Encrypt ➜ Sign` can allow an attacker to strip off<br>
the original signature and replace it with their own.


For symmetric encryption, `Sign ➜ Encrypt ➜ MAC`, which involves<br>
signing the message, appending the signature to the message, and<br>
using either `Encrypt ➜ MAC` or an **AEAD**, prevents this problem.


Similarly, if you’re forced to use asymmetric encryption, then you can still use `Sign ➜ Encrypt`<br>
but should include the [recipient’s name or the sender and recipient's names in the message][ Sign Encryption ]<br>
because the recipient needs proof that the same person signed and encrypted the message.

Once the signature and encryption layers are bound together, an attacker can't remove<br>
and replace the outer layer because the reference in the inner layer will reveal the tampering.

Alternatively, you can `Encrypt ➜ Sign ➜ Encrypt`<br>
or `Sign ➜ Encrypt ➜ Sign`, which are both slower.

---

「 **4** 」

**Don’t** use the same key pair for signatures (**Ed25519**) and key exchange (**X25519**)

It’s [recommended][ Recommendation ] to **never use the same key for more than one thing** in cryptography.

**The security of using the same key pair for these two algorithms**<br>
**has not been sufficiently studied**, signing key pairs and encryption<br>
key pairs often have different life cycles, and using different key pairs<br>
limits the damage done if one key pair is compromised.


Since the keys are so small, using different key<br>
pairs produces barely any overhead as well.


The only time you should really convert an **Ed25519** key pair<br>
to an **X25519** key pair is if you’re heavily [resource constrained][ Resource Constraints ]<br>
or when you’re forced to use **Ed25519** keys.

*(SSH public keys of GitHub could be used for hybrid encryption)*

---

「 **5** 」

**Prehash large messages**

Signing a message normally requires loading the entire message into<br>
memory, but this can be problematic for very large (**1+ GiB**) messages.

To solve this problem, you can use [Ed25519ph][ Multi Part Messages ] or **Ed448ph** (which probably isn't available)<br>
to perform the prehashing for you with some additional domain separation, or you can<br>
prehash the message yourself using a strong, modern hash function, like **BLAKE2b** or **SHA3**,<br>
with a **512-bit** output length and sign the hash instead of the message.

However, note that not prehashing means that<br>
**Ed25519** is [resistant to collisions][ Collisions Resistant ] in the hash function.

Therefore, when possible, ordinary signing should arguably be preferred for additional<br>
protection, *although this isn't realistically a problem if you use a secure hash function*.

---

「 **6** 」

Be aware of [fault attacks][ Fault Attacks ] against deterministic signatures

Techniques like causing [voltage glitches][ Voltage Glitches ] on a chip (Arduino)<br>
can be used to recover either the entire secret key or part of<br>
the secret key, depending on the signature algorithm, and<br>
create valid signatures with algorithms like **Ed25519**, **Ed448**<br>
and deterministic **ECDSA**.

However, this is primarily a concern on embedded devices<br>
and requires [physical or remote access][ Access Required ] to a device.

Four [countermeasures][ Countermeasures ] include signing the same data twice and comparing the outputs,<br>
which is obviously slower than signing once, verifying the signature after signing,<br>
which is [slower][ Signing ] than signing twice for small messages but [faster][ Signing ] for large messages,<br>
[calculating a checksum over input values][ Signing ] before and after signature generation,<br>
or using a cryptographic library that implements the algorithm with some random<br>
data in the calculation of the nonce, which is the technique used by [Signal][ Signal ].

However, these countermeasures are [not guaranteed to be effective][ Not Guaranteed ]<br>
and there can be other [side-channel attacks][ Side Channel Attack ] as well.

---

<br>
<br>

##### 「[ Overview ]」
