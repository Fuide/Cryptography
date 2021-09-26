
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
[ SHAcrypt ]: https://en.wikipedia.org/wiki/Crypt_(C)#SHA2-based_scheme
[ SHAcrypt Weaker ]: https://www.akkadia.org/drepper/SHA-crypt.txt

[ Length Extension Attack ]: https://en.wikipedia.org/wiki/Length_extension_attack
[ Password Derive Bytes ]: https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.passwordderivebytes?view=net-5.0
[ Many Iterations ]: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#pbkdf2

[ PBKDF1 Broken ]: https://crypto.stackexchange.com/questions/1842/how-does-pbkdf1-work
[ PBKDF ]: https://en.wikipedia.org/wiki/PBKDF2

[ Argon2i ]: https://en.wikipedia.org/wiki/Argon2
[ Argon2id ]: https://en.wikipedia.org/wiki/Argon2
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

[ Overview ]: ../Overview


# Key Derivation<br>Password-Based
##### 「[ Overview ]」

<br>
<br>
<br>

## **Recommended** 「 In Order 」

<br>

### [ Argon2id ]

`64+ MiB of RAM` `3+ Iterations` `1+ Parallelism`

Winner of the [Password Hashing Competition][ Password Hashing ] in 2015, widely used and<br>
recommended now, and very [ Easy To Use ] in libraries like **Libsodium**.

Use as high of a memory size as possible and then as many iterations<br>
as possible to reach a suitable delay for your use case, such as:
- A delay of **500ms** for server authentication
- **3** - **5** seconds for disk encryption
- **1s** for file encryption

---

### [ Scrypt ]

`N=32768, r=8, p=1 and higher`

The parameters are more confusing and less scalable<br>
than **Argon2**, and it’s susceptible to [ Cache Timing Attack ].

However, it’s still a [ Strong Algorithm ] when configured correctly.

---

### [ Bcrypt ]

`12+ Work Factor`

Note that **this is not a KDF** because the output length cannot be adjusted.<br>

**Only use this for password hashing when none of the better algorithms are available**

It’s better than **PBKDF2** in terms of [resisting GPU/ASIC attacks][ Strong Algorithm ],<br>
except for long passwords, but trickier to implement correctly<br>
and worse than **Argon2** and **Scrypt** in that it requires much<br>
less memory, with the amount of memory being fixed<br>
rather than adjustable.<br>

Unfortunately, it only uses the [first 55 characters of a password][ Strong Algorithm ] and has a<br>
stupid password length limit of [72 characters][ Maximum Password Length ], meaning people often prehash<br>
the password using something like **SHA2** to support longer passwords.

However, this can lead to [password shucking][ Password Shucking ] when using a weak hash function<br>
(**MD5**, which should **never** be used for anything anyway) and null bytes in the<br>
hash allowing an attacker to find [collisions][ Bcrypt Collisions ], speeding up attacks.

Therefore, you should [Base64 encode the prehash][ Base64 Pre Hash ] before passing it to **Bcrypt**.

---

### [PBKDF2-SHA512][ PBKDF ]

`120,000+ iterations`

**Only use this when none of the better algorithms are available**<br>
*or due to compatibility restraints*

Because it can be [efficiently bruteforced][ Strong Algorithm ] using GPUs<br>
and ASICs *when not using a high iteration count*.

Note that it’s generally recommended not to ask for more than the output<br>
length of the underlying hash function because this can lead to [attacks][ Output Length Attack ].

Instead, if that’s required, use **PBKDF2** first to get the output length of the<br>
underlying hash function (**64 bytes** with **PBKDF2-SHA512**) before calling a<br>
non-password-based **KDF**, like **HKDF-Expand**, with the **PBKDF2** output as<br>
the input keying material (**IKM**) to derive more output.

<br>
<br>
<br>

## **Avoid** 「 Unordered | All Unsuitable 」

<br>

### Storing Passwords In Plaintext

**This is a recipe for disaster**.<br>

If your password database is ever compromised, all your users are screwed,<br>
and your reputation in terms of security will go down the drain as well.

---

### Using Passwords As Keys

`Key = Encoding.UTF8.GetBytes( Password )`

Firstly, passwords are low in entropy, whereas **cryptographic keys need to be high in entropy**.

Secondly, not using a password-based **KDF** with a random salt means **attackers can quickly<br> bruteforce passwords** and users using the same password will end up using the same key.

---

### Using Regular / Fast hashes
**Such as ( [ MD5 ] | [ SHA1 ] | [ SHA2 ] )**

**These are not suitable for password hashing**<br>
*because they’re not slow, which allows for* ***fast bruteforce attacks***.

Password hashing also requires using a salt to protect<br>
against attacks using precomputed hashes and to prevent<br>
the same password always having the same hash.

However, adding a salt to certain regular hash functions, such as **SHA2**, can<br>
lead to [ Length Extension Attack ], as discussed in point **3** of [Hashing](./Hashing) notes.

---

### Encrypting Passwords

**Encryption is reversible, whereas hashing is not**

If an attacker compromises a password database and obtains a password<br>
hash, then they don’t know the password without computing the hash.

By contrast, if an attacker compromises a password database and the relevant<br>
encryption key(s), then they can easily obtain the plaintext passwords.

Encryption would also reveal the password length unless you padded the input.

---

### [PBKDF1][ PBKDF ]

**Never use this**

- As it was **superseded by PBKDF2**
- And can only derive keys up to **160-bits**,<br>
  which is basically not suitable for anything.

Some implementations, such as [PasswordDeriveBytes()][ Password Derive Bytes ] in **C#**, are also [completely broken][ PBKDF1 Broken ].

---

### [ SHAcrypt ]

- It's [weaker][ SHAcrypt Weaker ] than the recommended algorithms
- Nobody uses this
- And I’ve never even seen it in a cryptographic library.

---

###  PBKDF2 - ( [MD5][ PBKDF ] | [SHA1][ PBKDF ] | [SHA256][ PBKDF ] | [SHA384][ PBKDF ] )

**Use SHA512 if you must use PBKDF2**

**MD5** and **SHA1** are old hash functions that **should not be used anymore**.

Then **PBKDF2-SHA256** and **PBKDF2-SHA384** require [significantly more iterations][ Many Iterations ]<br>
than **PBKDF2-SHA512** to be secure and have a smaller block size, meaning long<br>
passwords may get prehashed.

---

### [Argon2i][ Argon2i ]

`Iterations < 3`

Unlike **Argon2id** and **Argon2d**, **Argon2i** has been [attacked][ Argon2 Analysis ],<br>
with [3+ iterations][ Argon2 Iterations ] being required for the attack to not be efficient<br>
and [11+ iterations][ Argon2 11 Iterations ] being required for the attack to completely fail.

**Argon2i** is also [weaker][ Argon2 Weaker ] than both **Argon2id** and **Argon2d**<br>
when it comes to resistance against GPU / ASIC cracking.

Therefore, as per the [RFC][ RFC9106 ], **Argon2id** should be used if you do not know the<br>
difference between the types or you consider side-channel attacks to be a<br>
viable threat because **Argon2id** offers the benefits of both **Argon2d**<br>
(GPU / ASIC resistance) and **Argon2i** (side-channel resistance, albeit to a lesser extent).

---

### Chained Hashing

`Scrypt( PBKDF2( Password ) )`

**This just reduces the strength of the stronger algorithm**<br>
since it means having worse parameters to get the same total delay.


<br>
<br>
<br>

## **Notes**

<br>

「 **1** 」

**Never hard-code passwords into source code**

These can be easily retrieved.

---

「 **2** 」

**Always use a random 128-bit or 256-bit salt**<br>
Salts ensure that each password hash is different, which prevents an attacker<br>from identifying two identical passwords without cracking the hashes.<br><br>
Moreover, salting defends against [attacks][ Password Cracking ] that rely on [precomputed hashes][ Rainbow Tables ].<br><br>
The typical salt size is **128-bits**, but **256-bit** is also fine for further reassurance that<br>
the salt won’t repeat. Anything above that is excessive, and short salts can lead to<br>
salt reuse and allow for precomputed attacks, which defeats the point of salting.

---

「 **3** 」

**Always use the highest parameters / delay you can afford**

Ideally, use a delay of **250+** milliseconds.<br>
In many cases, that’s too small.

For instance, **PBKDF2** requires a high number of iterations because it’s not resistant<br>
to GPU / ASIC attacks, and if you’re performing a non-interactive operation<br>
(disk encryption), then you can afford longer delays like **3** - **5** seconds.

---

「 **4** 」

**Avoid** string password variables

Strings are immutable in many programming languages such as **C#**,<br>
**Java**, **JavaScript** and **Go**, and thus can’t be zeroed out from memory.

Instead, use a char array if possible and convert that into a byte<br>
array for password hashing / password-based key derivation.

Then erase both arrays from memory after you’ve finished using them.

Note that this is also difficult in many programming languages, as<br>
explained in point **7** of the [Symmetric Encryption](./Symmetric%20Encryption) notes, but *attempting*<br>
to erase sensitive data from memory is better than doing nothing.

---

「 **5** 」

Compare passwords in **constant time**

If you ever need to compare passwords (for password re-entry in a console application),<br>
then you should use a constant time comparison function to prevent [timing attacks][ Timing Attacks ].

Sometimes these functions require both arrays to be equal in length, in which case you can<br>
hash both passwords using a regular hash function (**BLAKE2b-512**) for the comparison.<br>
Just erase these values from memory afterwards and don’t use them for anything else.

---

「 **6** 」

Use a **256-bit** and above output length

For password storage, a **128-bit** hash is normally fine, but a **256-bit**<br>
output provides a better security level for high entropy passwords.

For key derivation, you should derive at least a **256-bit** output and<br>
perhaps more, depending on whether you need to derive multiple<br>
keys (a **256-bit** encryption key and a **512-bit** **MAC** key).

---

「 **7** 」

**Always** store the parameters with the password hash<br>
*Such as `Memory Size`, `Iterations` and `Parallelism` for* ***Argon2***

These values don't need to be secret and are required to derive the correct hash.<br>

When storing passwords in a database, you should store these values for each user in order<br>
to verify the hashes and transition to stronger parameters over time as hardware improves.

In some [cryptographic libraries][ Done For You ], this is done for you.

By contrast, in a key derivation scenario, you can get away with using<br>
fixed parameters based on a version number stored as a header.

`File Format v3` = `256 MiB of RAM` + `12 Iterations`

Then if you want to change the parameters, you just increment the version number.

---

「 **8** 」

**Perform Client-side Password Prehashing**

For [server relief][ Server Relief ] or to [hide the plaintext password from the server][ Encryption Usage ]:<br>
*When creating an account, the server can send a* ***random*** *salt to the*<br>
*client that’s used to perform password hashing on the client’s device.*

The server then performs server-side password hashing<br>on the transmitted password hash using the same salt.

Then the salt and final password hash are stored in the password database.

When logging in, the server sends the stored salt to the client, the client performs<br>
client-side password hashing, the client transmits the password hash to the server,<br>
the server performs server-side password hashing using the stored salt, and then<br>
the server compares the result with the password hash stored in the database.

In the event of a non-existent user, the salt that’s sent should<br>
always be the same for a given username, which involves using<br>
a **MAC** (keyed **BLAKE2b-512**), with the username as the message.

---

「 **9** 」

**Don’t** use padding to hide the length of a password when sending it to a server

Instead, perform client-side password hashing if possible (please see point 8 above).

If that’s not possible, then you should hash the password using a regular<br>
hash function, with the largest possible output length (**BLAKE2b-512**), on<br>
the client’s device, transmit the hash to the server, and perform server-side<br>
password hashing, using the transmitted hash as the password.

Both techniques ensure that the amount of data transmitted is constant and<br>
prevent the server [effortlessly][ Keeping Passwords ] obtaining a copy of the password, but client-side<br>
password prehashing should be preferred as it allows for more secure password<br>
hashing parameters and provides additional security compared to if the server<br>
leaks / stores the client-side regular / fast hash of the password.

---

「 **10** 」

**Use** [rate limiting][ Rate Limiting ] to prevent denial of service (DOS) and bruteforce attacks

This involves blacklisting certain IP addresses and usernames from<br>
trying to log in temporarily to prevent the server being overwhelmed<br>
and to prevent attackers from bruteforcing passwords.

---

「 **11** 」

If a user can supply very long passwords, then this *can* lead to denial of service attacks<br>
*This happened to [Django][ Long Passwords ] in 2013.*

To fix this, either enforce a password length limit (**128** characters is the max)<br>
or prehash passwords using a regular / fast hashing algorithm, with the highest<br>
possible output length (**BLAKE2b-512**), before performing password hashing.

---

「 **12** 」

[Hash-then-Encrypt][ Hash Then Encrypt ] for additional security when storing passwords<br><br>

You can use a **password hashing algorithm** on the password before<br>
encrypting the salt and password hash using an **AEAD** or **Encrypt-then-MAC**,<br>
with a secret key **stored separately from the password database**.

This forces an attacker to decrypt the password hashes before trying to crack them.

Furthermore, it means that if your secret key is ever compromised but the<br>
password hashes are not, then you can decrypt all the stored password<br>
hashes and re-encrypt them using a new key, which is easier than resetting<br>
every user’s password in the event of a **pepper** being compromised.

---

「 **13** 」

Use a [pepper][ Pepper ] for additional security when deriving keys

<br>

A **pepper** is essentially a secret key that’s mixed with the password using a **MAC**

`HMAC-SHA512( Message : Password , Key : Pepper )`

before password hashing.

<br>

In the case of password storage, using `Hash ➜ Encrypt`<br>
makes more sense for the reason I explained above.

By contrast, for key derivation, using a **pepper** is a great idea if possible because<br>
it means an additional secret is required, making a bruteforce more difficult.

For instance, a keyfile in [file][ Key File ] / [ Disk ] encryption software acts as a **pepper**,<br>
which improves the security of the key derivation assuming that the keyfile<br>
is stored correctly (on an encrypted memory stick away from the encrypted file / disk).


---

<br>
<br>

##### 「[ Overview ]」
