
[ Libsodium ]: https://doc.libsodium.org/
[ Libsodium Audit ]: https://www.privateinternetaccess.com/blog/libsodium-v1-0-12-and-v1-0-13-security-assessment/
[ Visual C++ Redistributable ]: https://support.microsoft.com/sl-si/topic/the-latest-supported-visual-c-downloads-2647da03-1eea-4433-9aff-95f26a218cc0
[ Monocypher ]: https://monocypher.org/
[ Monocypher Audit ]: https://monocypher.org/quality-assurance/audit
[ Monocypher Speed ]: https://monocypher.org/speed

[ secretbox() ]: https://doc.libsodium.org/secret-key_cryptography/secretbox
[ secretstream() ]: https://doc.libsodium.org/secret-key_cryptography/secretstream

[ Tink ]: https://developers.google.com/tink
[ LibHydrogen ]: https://libhydrogen.org
[ Professional Mistakes ]: https://github.com/agl/ed25519/issues/27
[ NSEC ]: https://github.com/ektrah/nsec
[ OpenSSL ]: https://www.openssl.org/
[ OpenSSL Vulnerabilities ]: https://www.openssl.org/news/vulnerabilities.html
[ OpenSSL Forks ]: https://www.libressl.org/index.html
[ BearSSL ]: https://bearssl.org/goals.html
[ Programming Language ]: https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography?view=net-5.0
[ Bouncy Castle ]: https://bouncycastle.org/
[ CryptoJS ]: https://cryptojs.gitbook.io/docs/
[ CryptoJS Insecure ]: https://www.npmjs.com/package/evp_bytestokey
[ EVP_BytesToKey ]: https://www.openssl.org/docs/man1.1.1/man3/EVP_BytesToKey.html
[ PASETO ]: https://github.com/paragonie/paseto
[ NaCL ]: https://nacl.cr.yp.to/
[ NaCL Experimental ]: https://nacl.cr.yp.to/sign.html
[ Monocypher Why ]: https://monocypher.org/why
[ TweetNaCl ]: https://tweetnacl.cr.yp.to/
[ Gotchas ]: https://github.com/SalusaSecondus/CryptoGotchas
[ Post Quantum Cryptography ]: https://csrc.nist.gov/projects/post-quantum-cryptography



# Cryptographic Libraries

<br>
<br>
<br>
<br>

## **Recommended** 「 In Order 」

---

### [ Libsodium ]

- Modern
- Extremely fast
- Easy to use
- Well documented

[Audited][ Libsodium Audit ] library that covers all common use cases besides implementing **TLS**.<br>

However, it’s much bigger than **Monocypher**, which makes it<br>
harder to audit and thus not suitable for constrained environments.

<!-- Sometimes? -->
Sometimes it also requires the [ Visual C++ Redistributable ] to work on Windows.

---

### [ Monocypher ]

- Modern
- Easy to use
- Well documented

[Audited][ Monocypher Audit ] library, but it’s about [half][ Monocypher Speed ] the speed of **Libsodium** on desktops / servers.

Has no misuse resistant functions **Libsodium’s** [ secretstream() ] and [ secretbox() ].

Only supports **Argon2i** for password hashing, allowing for insecure parameters<br>
(please see the [Password Hashing/Password-Based Key Derivation](./Key%20Derivation%20-%20Password) Notes section)

Offers no

- Memory Locking
- Random Number Generation
- Base64 / Hex Encoding, Padding, ..

However, it’s compatible with **Libsodium** whilst being

- Fast
- Small
- Portable

making it useful for ***constrained environments*** like microcontrollers.

---

### [ Tink ]

A misuse resistant library that prevents common pitfalls like nonce reuse.

However, it doesn’t support *hashing* or *password hashing*, it’s not available<br>
in as many programming languages as **Libsodium** and **Monocypher**, and it<br>
provides *access to some algorithms that you* ***shouldn’t*** *use*.

---

### [ LibHydrogen ]

- Lightweight
- Easy to use
- Hard to misuse
- Well documented
- Suitable for most environments

The downsides are that it's not compatible with<br>
**Libsodium** whilst also running [slower][ Monocypher Speed ] than **Monocypher**.

However, it has some advantages over **Monocypher** like support<br>
for *random number generation*, even on **Arduino** boards,<br>and *easy access to key exchange patterns*, among other things.

---

<br>
<br>
<br>
<br>

## **Avoid** 「 In Order 」

---

### Random Github Libraries

`With 0 stars`

Assuming it’s not been written by an experienced professional and isn't a<br>
[**Libsodium** / **Monocypher** binding][ NSEC ] to another programming language,<br>
you should probably not use it.

Generally try to stay away from ***unpopular*** / ***unaudited*** libraries, as they<br>
are much more likely to be ***significantly slower*** and suffer from<br>
***vulnerabilities*** than the more *popular*, *audited* libraries.

Also, note that even [**experienced professionals** make mistakes][ Professional Mistakes ].

---

### [ OpenSSL ]

- Difficult to use / to use correctly
- The documentation is a mess
- Many [vulnerabilities][ OpenSSL Vulnerabilities ] have been found over the years
- Offers access to algorithms that you shouldn't use

These issues have led to **OpenSSL** [forks][ OpenSSL Forks ] and new, non-forked [libraries][ BearSSL ]<br>
that aim to be better alternatives in case you need to implement **TLS**.

---

### The Library In Your [ Programming Language ]

Most languages provide access to old algorithms like **MD5** and **SHA1**,<br>
that shouldn’t be used anymore, instead of newer ones such as<br>
**BLAKE2**, **BLAKE3** and **SHA3**, which can lead to poor algorithm choices.

Furthermore

- The ***APIs*** are typically easy to misuse
- The documentation may fail to mention important security related information
- And the implementations will likely be slower than **Libsodium**.

---

### 〔 [ Bouncy Castle ] 〕〔 [ CryptoJS ] 〕
**And other popular libraries**

These again often provide or rely on *dated algorithms* and typically have *bad documentation*.

For instance, **CryptoJS** uses an [insecure][ CryptoJS Insecure ] **KDF** called [EVP_BytesToKey()][ EVP_BytesToKey ] in **OpenSSL** when you<br>
pass a string password to `AES.encrypt()`, and **BouncyCastle** has no **C#** documentation.

However, this recommendation is too broad. Since there are *some* libraries that I haven't<br>
mentioned that are worth using, like [PASETO][ PASETO ], you can go with as a rule of thumb:

`If it doesn't include several of the algorithms I recommend, then it's probably bad`

*Just do your research and assess the quality of the documentation.*

---

### [NaCl][ NaCL ]

- Unmaintained
- Less modern
- More confusing version of **Libsodium** and **Monocypher**

For example, `crypto_sign()` for digital signatures has been [experimental][ NaCL Experimental ] for *several years*.<br>
It also doesn’t have *password hashing* support and is supposedly [difficult to install / package][ Monocypher Why ].

---

### [TweetNaCl][ TweetNaCl ]

- Unmaintained
- [Slower][ Monocypher Speed ] than **Monocypher**
- Doesn’t offer access to newer algorithms
- Doesn’t have *password hashing*
- [Doesn’t zero out buffers][ Monocypher Why ]

<br>
<br>
<br>
<br>

## **Notes**

<br>

1. If the library you’re currently using/planning to use doesn’t support several of the<br>
algorithms I’m recommending, then it’s time to upgrade and take advantage of the<br>
improved security and performance benefits available to you if you switch.

---

2. Please read the documentation<br><br>
Don’t immediately jump into coding something because that’s how mistakes are made.<br><br>
Good libraries have high quality documentation that will<br>explain potential security pitfalls and how to avoid them.

---

3. Some libraries release unauthenticated plaintext when using **AEADs**<br><br>
For example, **OpenSSL** and **BouncyCastle** [apparently do][ Gotchas ].<br><br>
Firstly, don’t use these libraries for this reason and the reasons I’ve already listed.<br>
Secondly **never do anything with unauthenticated plaintext; ignore it to be safe**.

---

4. Older does not mean better<br><br>
You can argue that older algorithms are more battle tested and therefore proven to be<br>
a safe choice, but the reality is that most modern algorithms, like **ChaCha20**, **BLAKE2**,<br>
and **Argon2**, have been properly analyzed at this point and shown to offer security and<br>
performance benefits over their older counterparts.<br><br>
Therefore, it doesn’t make sense to stick to this overly cautious mindset of<br>
avoiding newer algorithms, except for algorithms that are still candidates<br>
in a [competition][ Post Quantum Cryptography ] (e.g. new post-quantum algorithms), which do need<br>
further analysis to be considered safe.

---

5. You should prioritize speed<br><br>
This can make a noticeable difference for the user.<br><br>
For example, a **C#** **Argon2** library is not going to even come close to the speed of **Argon2**<br>
in **Libsodium**, meaning unnecessary and unwanted extra delay during key derivation.<br>
<br>**Libsodium** is the go-to for speed on desktops / servers, and **Monocypher**<br>
is the go-to for constrained environments (e.g. microcontrollers).
