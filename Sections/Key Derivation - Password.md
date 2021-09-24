
[ Argon2id ]: https://en.wikipedia.org/wiki/Argon2
[ Password Hashing ]: https://www.password-hashing.net/
[ Easy To Use ]: https://doc.libsodium.org/password_hashing/default_phf
[ Scrypt ]: https://en.wikipedia.org/wiki/Scrypt
[ Bcrypt ]: https://en.wikipedia.org/wiki/Bcrypt
[ Cache Timing Attack ]: https://crypto.stanford.edu/cs359c/17sp/projects/MarkAnderson.pdf
[ Strong Algorithm  ]: https://www.tarsnap.com/scrypt/scrypt.pdf
[ Maximum Password Length ]: https://en.wikipedia.org/wiki/Bcrypt#Maximum_password_length
[ Base64 Pre Hash ]: https://paragonie.com/blog/2015/04/secure-authentication-php-with-long-term-persistence
[ Bcrypt Collisions ]: https://blog.ircmaxell.com/2015/03/security-issue-combining-bcrypt-with.html
[ Password Shucking ]: https://www.youtube.com/watch?v=OQD3qDYMyYQ
[ Output Length Attack ]: https://blog.1password.com/1password-hashcat-strong-master-passwords/
[ MD5 ]: https://en.wikipedia.org/wiki/MD5
[ SHA1 ]: https://en.wikipedia.org/wiki/SHA-1
[ SHA2 ]: https://en.wikipedia.org/wiki/SHA-2
[ Length Extension Attack ]: https://en.wikipedia.org/wiki/Length_extension_attack
[ Password Derive Bytes ]: https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.passwordderivebytes?view=net-5.0
[ PBKDF1 Broken ]: https://crypto.stackexchange.com/questions/1842/how-does-pbkdf1-work
[ SHAcrypt ]: https://en.wikipedia.org/wiki/Crypt_(C)#SHA2-based_scheme
[ SHAcrypt Weaker ]: https://www.akkadia.org/drepper/SHA-crypt.txt
[ PBKDF ]: https://en.wikipedia.org/wiki/PBKDF2
[ Many Iterations ]: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#pbkdf2
[ Argon2i ]: https://en.wikipedia.org/wiki/Argon2
[ Argon2 Analysis ]: https://en.wikipedia.org/wiki/Argon2#Cryptanalysis
[ Argon2 Iterations ]: https://mailarchive.ietf.org/arch/msg/cfrg/beOzPh41Hz3cjl5QD7MSRNTi3lA/
[ Argon2 11 Iterations ]: https://eprint.iacr.org/2016/759.pdf
[ Argon2 Weaker ]: https://www.rfc-editor.org/rfc/rfc9106.html#name-security-against-time-space
[ RFC9106 ]: https://www.rfc-editor.org/rfc/rfc9106.html
[ Password Cracking ]: https://en.wikipedia.org/wiki/Password_cracking
[ Rainbow Tables ]: https://en.wikipedia.org/wiki/Rainbow_table
[ Timing Attacks ]: https://en.wikipedia.org/wiki/Timing_attack
[ Done For You ]: https://doc.libsodium.org/password_hashing/default_phf#password-storage
[ Server Relief ]: https://doc.libsodium.org/password_hashing#server-relief
[ Encryption Usage ]: https://bitwarden.com/help/article/what-encryption-is-used/#pbkdf2
[ Keeping Passwords ]: https://about.fb.com/news/2019/03/keeping-passwords-secure/
[ Long Passwords ]: https://www.djangoproject.com/weblog/2013/sep/15/security/
[ Rate Limiting ]: https://www.cloudflare.com/en-gb/learning/bots/what-is-rate-limiting/
[ Hash Then Encrypt ]: https://github.com/paragonie/password_lock#readme
[ Pepper ]: https://en.wikipedia.org/wiki/Pepper_(cryptography)
[ Key File ]: https://www.kryptor.co.uk/tutorial#using-a-keyfile
[ Disk ]: https://veracrypt.fr/en/Keyfiles%20in%20VeraCrypt.html


# Password Hashing/Password-Based Key Derivation

<br>
<br>
<br>

## Use 「 Ordered 」

<br>

### [Argon2id][ Argon2id ]

`64+ MiB of RAM, 3+ iterations, and 1+ parallelism`

Winner of the [Password Hashing Competition][ Password Hashing ] in 2015, widely used and recommended now, and very<br>
[easy to use][ Easy To Use ] in libraries like libsodium. Use as high of a memory size as possible and then as many<br>
iterations as possible to reach a suitable delay for your use case (e.g. a delay of 500 milliseconds<br>
for server authentication, 1 second for file encryption, 3-5 seconds for disk encryption, etc).

---

### [scrypt][ Scrypt ]

`N=32768, r=8, p=1 and higher`

The parameters are more confusing and less scalable than Argon2, and it’s susceptible to<br>
[cache-timing attacks][ Cache Timing Attack ]. However, it’s still a [strong algorithm][ Strong Algorithm ] when configured correctly.

---

### [bcrypt][ Bcrypt ]

`12+ work factor`

Note that **this is not a KDF** because the output length cannot be adjusted.<br>

**Only use this for password hashing when none of the better algorithms are available**.<br>

It’s better than PBKDF2 in terms of [resisting GPU/ASIC attacks][ Strong Algorithm ], except for long passwords (please keep reading),<br>
but trickier to implement correctly and worse than Argon2 and scrypt in that it requires much less memory,<br>
with the amount of memory being fixed rather than adjustable.<br>

Unfortunately, it only uses the [first 55 characters of a password][ Strong Algorithm ] and has a stupid password length limit of [72 characters][ Maximum Password Length ],<br>
meaning people often prehash the password using something like SHA2 to support longer passwords.<br>

However, this can lead to [password shucking][ Password Shucking ] when using a weak hash function (e.g. MD5, which should **never**<br>
be used for anything anyway) and null bytes in the hash allowing an attacker to find [collisions][ Bcrypt Collisions ], speeding up attacks.<br>

Therefore, you should [Base64 encode the prehash][ Base64 Pre Hash ] before passing it to bcrypt.

---

### [PBKDF2-SHA512][ PBKDF ]

`120,000+ iterations`

**Only use this when none of the better algorithms are available** or due to compatibility restraints <br>because it can be [efficiently bruteforced][ Strong Algorithm ] using GPUs and ASICs when not using a high iteration count.<br>

Note that it’s generally recommended not to ask for more than the output<br>length of the underlying hash function because this can lead to [attacks][ Output Length Attack ].<br>

Instead, if that’s required, use **PBKDF2** first to get the output length of the underlying hash function<br>
(64 bytes with **PBKDF2-SHA512**) before calling a non-password-based **KDF**, like **HKDF-Expand**,<br>
with the **PBKDF2** output as the input keying material (**IKM**) to derive more output.

<br>
<br>
<br>

## Avoid 「 Unordered | All Unsuitable 」

<br>

### Storing passwords in plaintext

**This is a recipe for disaster**.<br>

If your password database is ever compromised, all your users are screwed,<br>
and your reputation in terms of security will go down the drain as well.

---

### Using a password as a key

`key = Encoding.UTF8.GetBytes(password)`

Firstly, passwords are low in entropy, whereas **cryptographic keys need to be high in entropy**.<br>
Secondly, not using a password-based **KDF** with a random salt means **attackers can quickly<br> bruteforce passwords** and users using the same password will end up using the same key.

---

### Using a regular / fast hash function ( [MD5][ MD5 ] | [SHA1][ SHA1 ] | [SHA2][ SHA2 ] )

**These are not suitable for password hashing** because they’re not slow, which allows for **fast bruteforce attacks**.<br>
Password hashing also requires using a salt to protect against attacks using precomputed hashes and to prevent<br>
the same password always having the same hash. However, adding a salt to certain regular hash functions,<br>
such as **SHA2**, can lead to [length extension attacks][ Length Extension Attack ], as discussed in point 3 of the Notes in the [Hashing](./Hashing) section.

---

### Encrypting passwords

**Encryption is reversible, whereas hashing is not**.

If an attacker compromises a password database and obtains a password hash, then they don’t know the password<br>
without computing the hash. By contrast, if an attacker compromises a password database and the relevant<br>
encryption key(s), then they can easily obtain the plaintext passwords. Encryption would also<br>
reveal the password length unless you padded the input.

---

### [PBKDF1][ PBKDF ]

**Never use this** as it was **superseded by PBKDF2** and can only derive keys up to **160-bits**, which is basically not<br>suitable for anything. Some implementations, such as [PasswordDeriveBytes()][ Password Derive Bytes ] in **C#**, are also [completely broken][ PBKDF1 Broken ].

---

### [SHAcrypt][ SHAcrypt ]

It's [weaker][ SHAcrypt Weaker ] than the recommended algorithms, nobody uses this,<br>and I’ve never even seen it in a cryptographic library.

---

### [PBKDF2-MD5][ PBKDF ] | [PBKDF2-SHA1][ PBKDF ] | [PBKDF2-SHA256][ PBKDF ] | [PBKDF2-SHA384][ PBKDF ]

**Use SHA512 if you must use PBKDF2**.

**MD5** and **SHA1** are old hash functions that **should not be used anymore**.

Then **PBKDF2-SHA256** and **PBKDF2-SHA384** require [significantly more iterations][ Many Iterations ] than **PBKDF2-SHA512**<br>to be secure and have a smaller block size, meaning long passwords may get prehashed.

---

### [Argon2i][ Argon2i ]

`Iterations < 3`

Unlike **Argon2id** and **Argon2d**, **Argon2i** has been [attacked][ Argon2 Analysis ], with [3+ iterations][ Argon2 Iterations ] being required for the attack to not<br>be efficient and [11+ iterations][ Argon2 11 Iterations ] being required for the attack to completely fail. Argon2i is also [weaker][ Argon2 Weaker ] than both<br>Argon2id and Argon2d when it comes to resistance against GPU/ASIC cracking.

Therefore, as per the [RFC][ RFC9106 ], **Argon2id** should be used if you do not know the difference between the types<br>
or you consider side-channel attacks to be a viable threat because **Argon2id** offers the benefits of both<br>**Argon2i** (side-channel resistance, albeit to a lesser extent) and **Argon2d** (GPU/ASIC resistance).

---

### Chaining Hashing Functions

`scrypt( PBKDF2( password ) )`

**This just reduces the strength of the stronger algorithm** since<br>it means having worse parameters to get the same total delay.


<br>
<br>
<br>

## Notes

<br>

1. **Never hard-code passwords into source code**<br>
These can be easily retrieved.

---

2. **Always use a random 128-bit or 256-bit salt**<br>
Salts ensure that each password hash is different, which prevents an attacker<br>from identifying two identical passwords without cracking the hashes.<br><br>
Moreover, salting defends against [attacks][ Password Cracking ] that rely on [precomputed hashes][ Rainbow Tables ].<br><br>
The typical salt size is **128-bits**, but **256-bit** is also fine for further reassurance that<br>
the salt won’t repeat. Anything above that is excessive, and short salts can lead to<br>
salt reuse and allow for precomputed attacks, which defeats the point of salting.

---

3. **Always use the highest parameters/delay you can afford**<br><br>
Ideally, use a delay of **250+** milliseconds. In many cases, that’s too small. For instance,<br>**PBKDF2** requires a high number of iterations because it’s not resistant to GPU / ASIC <br>attacks, and if you’re performing a non-interactive operation (e.g. disk encryption),<br>
then you can afford longer delays like 3-5 seconds.

---

4. **Avoid** string password variables<br><br>
Strings are immutable (unchangeable) in many programming languages (e.g. **C#**, **Java**, **JavaScript**, **Go**, .. ),<br>
meaning they can’t be zeroed out from memory. Instead, use a char array if possible and convert that into<br>
a byte array for password hashing/password-based key derivation. Then erase both arrays from memory<br> after you’ve finished using them.<br><br>
Note that this is also difficult in many programming languages, as explained in point **7** of the [Symmetric Encryption](./Symmetric%20Encryption)<br>
Notes section, but *attempting* to erase sensitive data from memory is better than doing nothing.

---

5. Compare passwords in **constant time**<br><br>
If you ever need to compare passwords (e.g. for password re-entry in a console application),<br>
then you should use a constant time comparison function to prevent [timing attacks][ Timing Attacks ].<br><br>
Sometimes these functions require both arrays to be equal in length, in which case you can<br>
hash both passwords using a regular hash function (e.g. **BLAKE2b-512**) for the comparison.<br>
Just erase these values from memory afterwards and don’t use them for anything else.

---

6. Use a **256-bit** and above output length<br><br>
For password storage, a **128-bit** hash is normally fine, but a **256-bit**<br>
output provides a better security level for high entropy passwords.<br><br>
For key derivation, you should derive at least a **256-bit** output and<br>
perhaps more, depending on whether you need to derive multiple keys<br>
(e.g. a **256-bit** encryption key and a **512-bit** **MAC** key).

---

7. **Always** store the parameters (e.g. memory size, iterations, and parallelism for **Argon2**) with the password hash<br><br>
These values don't need to be secret and are required to derive the correct hash.<br>
When storing passwords in a database, you should store these values for each user in<br>
order to verify the hashes and transition to stronger parameters over time as hardware improves.<br><br>
In [some][ Done For You ] cryptographic libraries, this is done for you.<br><br>
By contrast, in a key derivation scenario, you can get away with<br>using fixed parameters based on a version number stored as a header.<br><br>
`file format v3 = 256 MiB of RAM and 12 iterations`<br><br>
Then if you want to change the parameters, you just increment the version number.

---

8. **Perform client-side password prehashing**<br><br>
For [server relief][ Server Relief ] or to [hide the plaintext password from the server][ Encryption Usage ]: when creating an account,<br>
the server can send a **random** salt to the client that’s used to perform password hashing on the client’s device.<br><br>
The server then performs server-side password hashing on the transmitted password hash using the same salt.<br>
Then the salt and final password hash are stored in the password database. When logging in, the server sends<br>
the stored salt to the client, the client performs client-side password hashing, the client transmits the password<br>hash to the server, the server performs server-side password hashing using the stored salt, and then the server compares the result with the password hash stored in the database.<br><br>
In the event of a non-existent user, the salt that’s sent should always be the same for a given username,<br>
which involves using a MAC (e.g. keyed **BLAKE2b-512**), with the username as the message.

---

9. **Don’t** use padding to hide the length of a password when sending it to a server<br><br>
Instead, perform client-side password hashing if possible (please see point 8 above). If that’s not possible, then<br>
you should hash the password using a regular hash function, with the largest possible output length (e.g. **BLAKE2b-512**),<br>
on the client’s device, transmit the hash to the server, and perform server-side password hashing, using the transmitted<br>
hash as the password.<br><br>
Both techniques ensure that the amount of data transmitted is constant and prevent the server [effortlessly][ Keeping Passwords ] obtaining<br>
a copy of the password, but client-side password prehashing should be preferred as it allows for more secure password<br>
hashing parameters and provides additional security compared to if the server leaks/stores the client-side regular/fast<br>
hash of the password.

---

10. **Use** [rate limiting][ Rate Limiting ] to prevent denial of service (DOS) and bruteforce attacks<br><br>
This involves blacklisting certain IP addresses and usernames from trying to log in temporarily<br>
to prevent the server being overwhelmed and to prevent attackers from bruteforcing passwords.

---

11. If a user can supply very long passwords, then this *can* lead to denial of service attacks: this happened to [Django][ Long Passwords ] in 2013.<br>
To fix this, either enforce a password length limit (e.g. **128** characters is the max) or prehash passwords using a regular/fast<br>
hashing algorithm, with the highest possible output length (e.g. **BLAKE2b-512**), before performing password hashing.

---

12. [Hash-then-Encrypt][ Hash Then Encrypt ] for additional security when storing passwords<br><br>
You can use a **password hashing algorithm** on the password before encrypting the salt and password hash using an **AEAD**<br>
or **Encrypt-then-MAC**, with a secret key **stored separately from the password database**. This forces an attacker to decrypt<br>
the password hashes before trying to crack them.<br><br>
Furthermore, it means that if your secret key is ever compromised but the password hashes are not,<br>
then you can decrypt all the stored password hashes and re-encrypt them using a new key, which is<br>
easier than resetting every user’s password in the event of a pepper being compromised.

---

13. Use a [pepper][ Pepper ] for additional security when deriving keys<br><br>
A pepper is essentially a secret key that’s mixed with the password using a **MAC**.<br><br>
`HMAC-SHA512( message : password , key : pepper )`<br><br>
Before password hashing. In the case of password storage, using Hash-then-Encrypt makes more sense<br>
for the reason I explained above. By contrast, for key derivation, using a pepper is a great idea if possible<br>
because it means an additional secret is required, making a bruteforce more difficult.<br><br>
For instance, a keyfile in [file][ Key File ] / [disk][ Disk ] encryption software acts as a pepper, which improves<br>
the security of the key derivation assuming that the keyfile is stored correctly<br>
(e.g. on an encrypted memory stick away from the encrypted file/disk).
