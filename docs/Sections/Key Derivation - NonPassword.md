
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

[ Overview ]: ../Overview


# Key Derivation Functions<br>Non-Password-Based
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

### [Salted BLAKE2b][ Key Derivation ]

[Restricted][ Blake 2 ] to a **128-bit** salt and **128-bit** (16 chars) personalization<br>parameter for domain separation, which is annoying.

However, you can feed more context information into the message parameter.

Besides the weird personal size limit, this is easier to use than **HKDF**<br>
because there’s only one function rather than three, which can be confusing.

Furthermore, please see the [Hashing](./Hashing) section for why<br>
**BLAKE2b** should be preferred over other hash functions.

If there's no **KDF** variant of **BLAKE2b** available in<br>
your library, then you should probably use **HKDF**.<br>
↳ `Please see below`

However, **if you know what you're doing**,<br>
you can construct a **BLAKE2b KDF** using:

```
BLAKE2b(
    Message : Salt | Info | Salt Length | Info Length
    Key : Input Keying Material
)
```

with the `Salt Length` and `Info Length` parameters being encoded<br>
as specified in point **5** of the [Message Authentication Codes](./Message%20Authentication) notes.

Like **HKDF**, this custom approach allows for salt<br>
and info parameters of practically any length.

---

### HKDF ( [SHA512][ HKDF ] | [SHA3-512][ HKDF ] )

The most popular **KDF** with support for a larger salt and lots of context information.

However, people get confused about the difference between<br>
the Expand and Extract functions, it doesn’t require a salt<br>
despite it being [recommended and beneficial for security][ RFC5869 ]<br>
and it’s slower than salted **BLAKE2b**.

Please see the [Hashing](./Hashing) and [Message Authentication Codes](./Message%20Authentication) sections<br>
for a comparison between **SHA2** / **SHA3** and **HMAC-SHA2** / **HMAC-SHA3**.

---

### [BLAKE3][ Blake 3 ]

As mentioned before, **BLAKE3** has a [lower security][ Blake 3 Specs ]<br>
margin,but it also doesn’t have a salt parameter.

With that said, [very good guidance][ Blake 3 ] is given on how to produce globally<br>
unique and application specific context strings in the [ Blake 3 Github ].

If you'd like a salt parameter, then you can construct a<br>
custom **KDF** implementation as explained in point **1** above.


<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

### Regular Hash Functions
**Salted & Unsalted**

Whilst this *can* be fine for deriving an encryption key from a **Diffie-Hellman**<br>
shared secret for example, it’s typically **not recommended**.

Just use an actual **KDF** when possible as there’s less that<br>
can go wrong (there's no risk of [length extension attacks][ Length Extension Attack ]).

---

### Password-based KDFs
**Such as [PBKDF2][ PBKDF2 ]**

If you’re not using a password, then you shouldn’t be using a password-based KDF.

Password-based **KDFs** are designed to be slow to prevent [bruteforce attacks][ Bruteforce Attack ], whereas<br>
non-password-based **KDFs** are fast because they're designed for high-entropy keys.

Even with a small delay (1 iteration of **PBKDF2**), this is likely slower and makes<br>
the code more confusing because an inappropriate function is being used.

---

### 〔 [HChaCha20][ Nonce Extension ] 〕  〔 [HSalsa20][ XSalsa ] 〕
**These are not general-purpose cryptographic hash functions**

- Can only take a **256-bit** key as input
- And output a **256-bit** key
- Are very rarely used<br>
  Except in the case of implementing **XChaCha20** and **XSalsa20**

If you want something based on **ChaCha**, then use **BLAKE2b** or **BLAKE3**.


<br>
<br>
<br>

#### **Notes**

<br>

「 **1** 」

**These KDFs are not suitable for hashing passwords**

They should be used for deriving multiple subkeys from<br>
a **high-entropy** master key or converting a shared secret<br>
into a cryptographically strong secret key.

---

「 **2** 」

Using the same parameters besides changing the output<br>
length can result in related outputs (for **HKDF** and **BLAKE3**)

***This is exactly why you shouldn’t reuse the same parameters for different keys.***

---

「 **3** 」

Use different contexts for different keys

A good format is `[ Application ] [ Date & Time] [ Purpose ]`<br>
because this means the context information is application-specific<br>
and unique, which provides domain separation.

---

「 **4** 」

Salted **BLAKE2b** can use a **counter** salt

If you’re deriving multiple subkeys from a master key,<br>
then you can use a counter salt starting at **0** (**16 bytes** of **0s**)<br>
that gets incremented for each subkey.

However, if you’re deriving a single key from a<br>
shared secret, then you should use a random salt.

---

「 **5** 」

Use a **random** salt with **HKDF** when possible

The [RFC][ RFC5869 ] explains that using a random salt adds to the security of the scheme.

The authors recommend a salt as long as the hash<br>
output length, but a **128-bit** or **256-bit** salt is sufficient.

Using a secret salt (a bit like a [ Pepper ]) further improves the security guarantees.

---

<br>
<br>

##### 「[ Overview ]」
