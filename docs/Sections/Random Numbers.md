
[ RNGCryptoServiceProvider ]: https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rngcryptoserviceprovider?view=net-5.0
[ Fast Key Erasure ]: https://blog.cr.yp.to/20170723-random.html
[ CHACHA20 Random ]: https://github.com/WebAssembly/wasi-libc/blob/main/libc-top-half/sources/arc4random.c

[ Overview ]: ../Overview


# Random Numbers
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

<br>

### CSPRNG

The **cryptographically secure** pseudorandom number generator<br>
in your programming language or cryptographic library.

These should use the operating system’s **CSPRNG**.<br>
For example, [ RNGCryptoServiceProvider ] in **C#**.

---

### [Fast Key Erasure][ Fast Key Erasure ]
**On Embedded Systems**

This should be ***a last resort*** because it’s hard to erase keys properly.

***A lot can go wrong if you don’t know what you’re doing***.

[Here's][ CHACHA20 Random ] an example **ChaCha20 RNG** implementation.

<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

<br>

### Non-CSPRNG
**Non-cryptographically secure** pseudo random number generator

<br>

For example:

<table>
    <tr>
        <td><b>JavaScript</b></td>
        <td><a href = 'https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random'>Math.random()</a></td>
    </tr>
    <tr>
        <td><b>Java</b></td>
        <td><a href = 'https://docs.oracle.com/javase/8/docs/api/java/util/Random.html'>Random()</a></td>
    </tr>
    <tr>
        <td><b>C#</b></td>
        <td><a href = 'https://docs.microsoft.com/en-us/dotnet/api/system.random.next?view=net-5.0'>Random.Next()</a></td>
    </tr>
</table>

<br>

***These are not secure and should not be used for anything related to security***.

---

### Custom RNGs

These are **likely** going to be **insecure** because<br>it’s harder to do, properly, than you’d think.

**Just trust the operating system’s CSPRNG**.
