
[ Key Length 3 ]: https://www.keylength.com/en/3/
[ Key Length 6 ]: https://www.keylength.com/en/6/

[ Threefish ]: https://en.wikipedia.org/wiki/Threefish
[ Why 256 ]: https://blog.1password.com/why-we-moved-to-256-bit-aes-keys/

[ Overview ]: ../Overview


# Symmetric Key Size
##### 「[ Overview ]」

<br>
<br>
<br>


## **Recommended** 「 Unordered | Distinct Use-Cases 」

<br>

### 256-bit

There’s essentially no reason not to use **256-bit** keys for symmetric encryption.


This is the only available key size for most **(X)ChaCha20** and **(X)Salsa20** implementations,<br>
it’s the key size that’s used for [top secret material][ Key Length 6 ] by intelligence agencies and governments,<br>
and it’s [now recommended][ Key Length 3 ] for long-term storage due to concerns surrounding quantum<br>
computers being able to bruteforce **128-bit** keys.

---

### 512-bit

If you’re using a **MAC** like **HMAC-SHA512** or keyed<br>
**BLAKE2b-512**, then you should use a **512-bit** key.

This helps with domain separation when deriving keys, and it’s recommended to always use<br>
a key size as large as the output length for **HMAC** (e.g. a **256-bit** key for **HMAC-SHA256**).

This ensures that the key size doesn't decrease the security provided by the **MAC**.


<br>
<br>
<br>


## **Avoid** 「 In Order 」

<br>

### Smaller than 128-bit keys

This won’t stand the test of time and in some cases can already be bruteforced.

---

### Symmetric Encryption + Large Keys
**Such As [ Threefish ]**

Anything over **256-bit** is currently regarded as unnecessary.<br>
Furthermore, encryption algorithms supporting such key sizes are very unpopular in practice.

Note that the situation is different for **MACs**, as explained in point **2** of the Use section above.

---

### 128-bit

**This is the minimum**, but please just use **256-bit** keys because<br>
they provide a higher security margin for an insignificant cost.

The [argument][ Why 256 ] that **AES-128** is more secure than **AES-256** due<br>
to certain attacks being more effective on **AES-256** is incorrect<br>
because such attacks are **not** practical in the real world.

You should ideally use **ChaCha20** instead of **AES** anyway since<br>
it has a higher security margin and runs in constant time, avoiding<br>
timing attacks, as explained in the [Symmetric Encryption](./Symmetric%20Encryption) Use section.

---

<br>
<br>

##### 「[ Overview ]」
