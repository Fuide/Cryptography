
[ Creative Commons ]: https://i.creativecommons.org/l/by-sa/4.0/88x31.png
[ Inspiration ]: https://gist.github.com/atoponce/07d8d4c833873be2f68c34f9afc5a78a#file-gistfile1-md
[ Latacora ]: https://latacora.singles/2018/04/03/cryptographic-right-answers.html
[ Gotchas ]: https://github.com/SalusaSecondus/CryptoGotchas
[ Libsodium ]: https://doc.libsodium.org/
[ TLS1.3 ]: https://www.davidwong.fr/tls13/
[ Noise Protocol ]: https://noiseprotocol.org/noise.html
[ WireGuard ]: https://www.wireguard.com/protocol/
[ How to Learn About Cryptography ]: https://samuellucas.com/blog/how-to-learn-about-cryptography.html
[ Sha3 ]: https://competitions.cr.yp.to/sha3.html
[ Competition ]: https://competitions.cr.yp.to/index.html

[ Simple Implementation ]: https://datatracker.ietf.org/doc/html/rfc5869
[ Difficult Implementation ]: https://loup-vaillant.fr/articles/implementing-elligator
[ Horrible Documentation ]: https://www.openssl.org/docs/

[ Basic HTML ]: https://nacl.cr.yp.to/index.html
[ A Bunch Of Files On GitHub ]: https://github.com/google/tink/tree/master/docs
[ Cryptography Stack Exchange ]: https://crypto.stackexchange.com/
[ GitBook ]: https://doc.libsodium.org/

[ Professional Mistakes ]: https://github.com/agl/ed25519/issues/27
[ Missed Mistakes ]: https://github.com/str4d/rage/issues/195

[ License ]: https://creativecommons.org/licenses/by-sa/4.0/

[ Contact Me ]: https://github.com/samuel-lucas6/Cryptography-Guidelines/issues
[ This Version ]: https://github.com/Fuide/Cryptography
[ Original Git ]: https://github.com/samuel-lucas6/Cryptography-Guidelines
[ Original ]: https://github.com/samuel-lucas6/Cryptography-Guidelines

[ Blog ]: https://soatok.blog/



<div align='center'>
    <p>
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Libraries'>Libraries</a> ⦘</b> 
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Random%20Numbers'>Random Numbers</a> ⦘</b> 
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Symmetric%20Encryption'>Symmetric Encryption</a> ⦘</b> 
        <br>
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Hashing'>Hashing</a> ⦘</b> 
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Digital%20Signatures'>Digital Signatures</a> ⦘</b> 
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Message%20Authentication'>Message Authentication</a> ⦘</b> 
        <br>
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Hybrid%20Encryption'>Key Exchange / Hybrid Encryption</a> ⦘</b> 
    </p>
    <b>Ｋｅｙ　Ｄｅｒｉｖａｔｉｏｎ</b>
    <p>
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Key%20Derivation%20-%20Password'>Password Based</a> ⦘</b> 
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Key%20Derivation%20-%20NonPassword'>Non-Password Based</a> ⦘</b>
    </p>
    <b>Ｋｅｙ　Ｓｉｚｅ</b>
    <p>
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Symmetric%20Keys'>Symmetric</a> ⦘</b> 
        <b>⦗ <a href='https://fuide.github.io/Cryptography/docs/Sections/Asymmetric%20Keys'>Asymmetric</a> ⦘</b>
    </p>
</div>


---


## Background

This document outlines recommendations for ***cryptographic algorithm choices*** and<br>
parameters as well as ***important implementation details*** based on what I have<br>
learned from reading about the subject and the consensus I have observed online.

*Note that some knowledge of cryptography is required*<br>
*to understand the terminology used in this guide.*

**Goal**<br>
My goal with these guidelines is to provide a resource that I wish I had<br>
access to when I first started writing programs related to cryptography.

If this information helps ***prevent even just one vulnerability***, then I consider it time well spent.

---

## Acknowledgments

These guidelines were inspired by:
- [Cryptographic Best Practices Gist][ Inspiration ]
- [Latacora's Cryptographic Right Answers][ Latacora ]
- [Crypto Gotchas][ Gotchas ] [**CC 4.0 License**]( License )

This guide distinguishes itself through mentions of **newer algorithms**,<br>
***justifications for algorithm recommendations*** whilst also offering<br>
important information on ***how to use them correctly***.

---

## Disclaimer

I’m a psychology undergraduate with an interest in applied cryptography,<br>
not an experienced cryptographer.

I primarily have experience with [ Libsodium ]<br>
since that’s what I’ve used for my projects,<br>
but I've also reported some security<br>
vulnerabilities related to cryptography.

Most experienced cryptographers don't have the time to write things like this,<br>
and the following information is freely available online or in books, so whilst<br>
more experience would be beneficial,

***I’m trying my best to provide accurate information that can be fact checked.***

<br>

##### **If I've made a mistake, please [ Contact Me ] to get it fixed**.

<br>
<br>

Note that the rankings are based on:
- My opinion
- Availability in cryptographic libraries
- Typically used in modern protocols:
    - [ TLS1.3 ]
    - [Noise Protocol Framework][ Noise Protocol ]
    - [ WireGuard ]

<br>

Such protocols and recommended practices make for the best guidelines<br>
because they’ve been approved by **experienced professionals**.

---

## General Guidance

<br>

### Research
*Research, Research, Research!*

You often don’t need to know how cryptographic algorithms<br>
work under the hood to implement them correctly, just like<br>
how you don’t need to know how a car works to drive.

However, you need to know enough about what you’re trying to do,<br>
which requires looking up relevant information **online** or in **books**,<br>
reading the **documentation** for the cryptographic library you’re using,<br>
**RFC** standards, helpful **blog posts** and **guides** like this one.

Furthermore, reading books about the subject in general will be beneficial,<br>
again like how knowing about cars can help if you break down.

For a list of great resources, check out my [ How to Learn About Cryptography ] blog post.

<br>

### Test
*And Test Again*

It’s your responsibility to get things right the first time around<br>
to the best of your ability rather than relying on peer reviews.

Therefore, ***I strongly recommend*** always reading over security sensitive<br>
code at least twice and testing it to ensure that it’s operating as expected.<br>
↳ `Checking code line by line using a debugger, using test vectors, ...`

<br>

### Peer Reviews
*Are Great But Often Don’t Happen*

Unless your project is popular, you have a bug bounty program with cash<br>
rewards, or what you’re developing is for an organization, very few people,<br>
perhaps none at all, will look through the code to find and report vulnerabilities.

Similarly, receiving funding for a code audit will probably be near impossible.

<br>

### Custom Algorithms
*Please don't create your own custom cryptographic algorithms for* ***use in production***

This is like flying a Boeing 747 without a pilot license but worse because even experienced<br>
cryptographers design [insecure algorithms][ Sha3 ], which is why cryptographic algorithms are<br>
thoroughly analyzed by a large number of cryptanalysts, usually as part of a [ Competition ].

By contrast, you rarely see experienced airline pilots crashing planes.

The only **exception** to this rule is implementing something<br>
like `Encrypt ➜ MAC` with secure, **existing** cryptographic<br>
algorithms ***when you know what you're doing***.

<br>

### Recoding Algorithms
*Please don't recreate existing cryptographic algorithms for* ***use in production***

Cryptographic libraries provide access to these algorithms for you to prevent people<br>
from making mistakes that cause vulnerabilities and to offer good performance.

Whilst a select few algorithms are relatively simple to implement, like [HKDF][ Simple Implementation ],<br>
[many aren't][ Difficult Implementation ] and require a great deal of experience to implement correctly.

It also doesn't help that most academic papers and reference<br>
implementations are quite difficult to understand.

---

## Concluding Remarks

I believe there are **three main areas of improvement** when it comes<br>
to individuals with experience in cryptography helping developers.

<br>

### Cryptographic Libraries
**Should** Be Better

Most don’t make it easy to use cryptography safely (they support insecure<br>
    algorithms, require nonces, etc) and have [ Horrible Documentation ].

<br>

**This shouldn’t be the case**, and there really ***should be greater uproar about this.***

<br>

- Things need to be **secure by default**
- **Insecure algorithms** should never be implemented or get removed
- **Documentation** needs to be readable, as in:
    - Concise
    - Helpful to people of all skill levels
    - Presented on a modern looking website rather than<br>
      using [ Basic HTML ] or [ A Bunch Of Files On GitHub ]
    - Easy to navigate ➜ Support search functionality like [ GitBook ] does

<br>

### Stop Saying
`Don't roll your own crypto`

<br>

**Repeating this phrase doesn't help anyone**.

<br>

Instead **educate developers** about how to do things properly, whether that be by:
- Answering questions on [ Cryptography Stack Exchange ] in an understandable manner
- Writing a [ Blog ]
- Replying to emails from people asking for help

<br>

It's not a crime to implement `Encrypt ➜ MAC`,<br>
and even when someone writes a custom cipher,<br>
**you should explain why that's not a good idea**<br>
↳`professional cryptographers still design insecure algorithms`

<br>

### Peer Reviews
**Should** Be Done More Often

<br>

It's often:
- Difficult to receive peer review
- Impossible to fund a bug bounty program with cash rewards
- And extremely unlikely for projects to get funding for a code audit.

<br>

Whilst developers who fail to do any reading related to cryptography<br>
obviously deserve criticism, [Even Experienced Professionals Make Mistakes][ Professional Mistakes ].

Simple peer review, using the search on GitHub for things like **HMAC** and **ECB**,<br>
helps catch things that are easy to spot, and more thorough peer review helps<br>
catch things that even someone experienced [Might Have Missed][ Missed Mistakes ].

**If something seems dodgy, then you should investigate if possible.**

<br>

---

<br>

### Thank you
For reading this guide, if you found it useful, ***consider visiting the [ Original Git ]***.

`I hope you liked my edits / formatting`

**Please consider sharing** [ This Version ] or the [ Original ] so others can find them easier.
