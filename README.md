
[ License ]: https://creativecommons.org/licenses/by-sa/4.0/
[ Creative Commons ]: https://i.creativecommons.org/l/by-sa/4.0/88x31.png
[ Inspiration ]: https://gist.github.com/atoponce/07d8d4c833873be2f68c34f9afc5a78a#file-gistfile1-md
[ Latacora ]: https://latacora.singles/2018/04/03/cryptographic-right-answers.html
[ Gotchas ]: https://github.com/SalusaSecondus/CryptoGotchas
[ Libsodium ]: https://doc.libsodium.org/
[ TLS1.3 ]: https://www.davidwong.fr/tls13/
[ Noise Protocol ]: https://noiseprotocol.org/noise.html
[ WireGuard ]: https://www.wireguard.com/protocol/
[ Learn Cryptography ]: https://samuellucas.com/blog/how-to-learn-about-cryptography.html
[ Sha3 ]: https://competitions.cr.yp.to/sha3.html
[ Competition ]: https://competitions.cr.yp.to/index.html
[ Simple Implementation ]: https://datatracker.ietf.org/doc/html/rfc5869
[ Difficult Implementation ]: https://loup-vaillant.fr/articles/implementing-elligator
[ Horrible Documentation ]: https://www.openssl.org/docs/
[ Basic HTML ]: https://nacl.cr.yp.to/index.html
[ Github Files ]: https://github.com/google/tink/tree/master/docs
[ Stack Exchange ]: https://crypto.stackexchange.com/
[ Search Functionality ]: https://doc.libsodium.org/
[ Professional Mistakes ]: https://github.com/agl/ed25519/issues/27
[ Missed Mistakes ]: https://github.com/str4d/rage/issues/195
[ Cryptography Guidelines ]: https://github.com/samuel-lucas6/Cryptography-Guidelines
[ Contribute ]: ./CONTRIBUTE.md


# Cryptography Implementation Guide
#### 「[ License ]」 「[ Contribute ]」

---

This is a **formatted** version of the 「[ Cryptography Guidelines ]」

---

## Background

This document outlines recommendations for cryptographic algorithm choices and parameters as well as important implementation details based on what I have learned from reading about the subject and the consensus I have observed online. Note that *some* knowledge of cryptography may be required to understand the terminology used in these guidelines.

My goal with these guidelines is to provide a resource that I wish I had access to when I first started writing programs related to cryptography. If this information helps prevent even just one vulnerability, then I consider it time well spent.

---

## Acknowledgments

These guidelines were inspired by [this][ Inspiration ] Cryptographic Best Practices gist, Latacora's [Cryptographic Right Answers][ Latacora ], and [Crypto Gotchas][ Gotchas ], which is licensed under the [Creative Commons Attribution 4.0 International License][ License ]. The difference is that I mention newer algorithms and have tried to justify my algorithm recommendations whilst also offering important notes about using them correctly.

---

## Disclaimer

I’m a psychology undergraduate with an interest in applied cryptography, not an experienced cryptographer. I primarily have experience with the [libsodium][ Libsodium ] library since that’s what I’ve used for my projects, but I've also reported some security vulnerabilities related to cryptography.

Most experienced cryptographers don't have the time to write things like this, and the following information is freely available online or in books, so whilst more experience would be beneficial, I’m trying my best to provide accurate information that can be fact checked. **If I've made a mistake, please contact me to get it fixed**.

Note that the rankings are based on my opinion, algorithm availability in cryptographic libraries, and which algorithms are typically used in modern protocols, such as [TLS 1.3][ TLS1.3 ], [Noise Protocol Framework][ Noise Protocol ], [WireGuard][ WireGuard ], and so on. Such protocols and recommended practices make for the best guidelines because they’ve been approved by experienced professionals.

---

## General Guidance

1. Research, research, research: you often don’t need to know how cryptographic algorithms work under the hood to implement them correctly, just like how you don’t need to know how a car works to drive. However, you need to know enough about what you’re trying to do, which requires looking up relevant information online or in books, reading the documentation for the cryptographic library you’re using, reading RFC standards, reading helpful blog posts, and reading guidelines like this one. Furthermore, reading books about the subject in general will be beneficial, again like how knowing about cars can help if you break down. For a list of great resources, check out my [How to Learn About Cryptography][ Learn Cryptography ] blog post.

2. Check and check again: it’s your responsibility to get things right the first time around to the best of your ability rather than relying on peer review. Therefore, I **strongly** recommend always reading over security sensitive code at least twice and testing it to ensure that it’s operating as expected (e.g. checking the value of variables line by line using a debugger, using test vectors, etc).

3. Peer review is great but often doesn’t happen: unless your project is popular, you have a bug bounty program with cash rewards, or what you’re developing is for an organization, very few people, perhaps none at all, will look through the code to find and report vulnerabilities. Similarly, receiving funding for a code audit will probably be near impossible.

4. **Please don't create your own custom cryptographic algorithms (e.g. a custom cipher or hash function)**: this is like flying a Boeing 747 without a pilot license but worse because even experienced cryptographers design [insecure][ Sha3 ] algorithms, which is why cryptographic algorithms are thoroughly analyzed by a large number of cryptanalysts, usually as part of a [competition][ Competition ]. By contrast, you rarely see experienced airline pilots crashing planes. The only *exception* to this rule is implementing something like Encrypt-then-MAC with secure, **existing** cryptographic algorithms **when you know what you're doing**.

5. **Please avoid coding existing cryptographic algorithms yourself (e.g. coding AES yourself)**: cryptographic libraries provide access to these algorithms for you to prevent people from making mistakes that cause vulnerabilities and to offer good performance. Whilst a select few algorithms are relatively simple to implement, like [HKDF][ Simple Implementation ], [many aren't][ Difficult Implementation ] and require a great deal of experience to implement correctly. Lastly, another reason to avoid doing this is that it's really not fun since academic papers and reference implementations can be very difficult to understand.

---

## Overview

- [ Cryptographic Libraries ]( ./Sections/Libraries.md )
- [ Symmetric Encryption ]( ./Sections/Symmetric%20Encryption.md )
- [ Message Authentication ]( ./Sections/Message%20Authentication.md )
- [ Random Numbers ]( ./Sections/Random%20Numbers.md )
- [ Hashing ]( ./Sections/Hashing.md )
- [ Key Exchange / Hybrid Encryption ]( ./Sections/Hybrid%20Encryption.md )
- [ Digital Signatures ]( ./Sections/Digital%20Signatures.md )
- **Key Derivation**
    - [ Password Based ]( ./Sections/Key%20Derivation%20-%20Password.md )
    - [ Non-Password ]( ./Sections/Key%20Derivation%20-%20NonPassword.md )
- **Key Sizes**
    - [ Symmetric ]( ./Sections/Symmetric%20Keys.md )
    - [ Asymmetric ]( ./Sections/Asymmetric%20Keys.md )

---

## Concluding Remarks

I believe there are three main areas of improvement when it comes to individuals with experience in cryptography helping developers:

1. Cryptographic libraries **should** be better: most don’t make it easy to use cryptography safely (e.g. they support insecure algorithms, require nonces, etc) and have [horrible documentation][ Horrible Documentation ]. This shouldn’t be the case, and there really should be greater uproar about this. Things need to be secure by default (e.g. insecure algorithms should never be implemented or get removed), and the documentation needs to be readable, as in concise, helpful to people of all skill levels, presented on a modern looking website rather than using [basic HTML][ Basic HTML ] or [a bunch of files on GitHub][ Github Files ], and easy to navigate (e.g. supporting search functionality like [GitBook][ Search Functionality ] does).

2. People **should** stop saying 'don't roll your own crypto': **repeating this phrase doesn't help anyone**. Instead, educate developers about how to do things properly, whether that be by answering questions on [Cryptography Stack Exchange][ Stack Exchange ] in an understandable manner, writing a [blog](https://soatok.blog/), or replying to emails from people asking for help. It's not a crime to implement Encrypt-then-MAC, and even when someone writes a custom cipher, **you should explain why that's not a good idea** (e.g. 'professional cryptographers still design insecure algorithms').

3. There **should** be more peer review: it's often difficult to receive peer review, impossible to fund a bug bounty program with cash rewards, and extremely unlikely for projects to get funding for a code audit. Whilst developers who fail to do any reading related to cryptography obviously deserve criticism, [even experienced professionals make mistakes][ Professional Mistakes ]. Simple peer review (e.g. using the search on GitHub for things like 'HMAC' and 'ECB') helps catch things that are easy to spot, and more thorough peer review helps catch things that even someone experienced [might have missed][ Missed Mistakes ]. If something seems dodgy, then you should investigate if possible.
