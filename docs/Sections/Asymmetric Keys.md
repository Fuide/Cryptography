
[ Key Length 3 ]: https://www.keylength.com/en/3/
[ Key Length 6 ]: https://www.keylength.com/en/6/

[ Elliptic Curves ]: https://en.wikipedia.org/wiki/Elliptic-curve_cryptography
[ Diffie Hellman ]: https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman
[ TLS Implementation ]: https://en.wikipedia.org/wiki/Comparison_of_TLS_implementations#Supported_elliptic_curves

[ Overview ]: ../Overview


# Asymmetric Key Size
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

### 256-bit

The key size for **X25519**, which provides a `~128-bit security` level.<br>
Why am I recommending this when I recommend 256-bit keys,<br>
a `256-bit security` level, for symmetric encryption?<br>
<br>
Because **X25519** is faster, more common, and [more accessible][ TLS Implementation ] than X448.<br>
<br>
If quantum computers do come along, then [ECC][ Elliptic Curves ] and RSA will be broken regardless<br>
of the key size anyway, so many people feel less of a need to use a higher<br>
security level curve considering that `128-bit security` is currently enough.

---

### 456-bit

The key size for **X448**, which provides a `224-bit security` level.

---

### 3072-bit / 4096-bit

**If you’re forced to use RSA**, then the minimum key size should be **3072-bit**,<br>
which is the key size [currently used by the NSA][ Key Length 6 ] and recommended by ECRYPT<br>
for [near term protection][ Key Length 3 ].<br>
<br>
The maximum should be **4096-bit** because the performance is really bad after that.<br>
However, seriously **don’t use RSA!**


<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

### 1024-bit

These are **no longer secure**.

---

### 2048-bit

These only provide a `112-bit security` level, which is below the standard<br>
`128-bit security` level. Therefore, whilst commonly used and still safe as<br>
a **minimum** RSA key size, it makes sense to use **3072-bit** keys instead.

---

### 8192-bit

These are slow to generate and excessive to store.

---

### Post-quantum Algorithm Key Sizes

These algorithms are still being researched, and the<br>
key sizes are very large compared to those for [ECDH][ Diffie Hellman ].

---

<br>
<br>

##### 「[ Overview ]」
