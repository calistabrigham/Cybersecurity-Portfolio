Title: Byte-size RSA - Solution
Author: Calista Brigham

First, note that the title indicates that the challenge is about RSA. RSA (which stands for Rivest-Shamir-Adleman) is an asymmetric encryption standard. This means that it uses two keys -- a private key and a public key. This is in contrast to symmetric encryption, which uses the same key for both encryption and decryption. RSA's applications include message encryption and digital signatures.

RSA relies on a set of 6 numbers, represented by the variables *p*, *q*, *n*, *$\Phi$(n)*, *e*, and *d*, as described below.
- *p* and *q*: Secret prime numbers used to calculate *n* and *$\Phi$(n)*
- *n*: The known modulus value; equal to `p * q`
- *$\Phi$(n)*: A secret number used to calculate *d*; equal to `(p-1) * (q-1)`
- *e*: The known (public) exponent, which should also be a prime number; certain values (such as 65537) are common
- *d*: The secret (private) exponent, such that `d * e % $\Phi$(n) = 1` (i.e., *d* and *e* are modular multiplicative inverses)

After all the values are calculated, *n* and *e* are paired to become the public key, and *n* and *d* are paired to become the private key. With a plaintext message *m* and a ciphertext *c*, encryption and decryption occur as follows:
- Encryption: `c = m^e % n`
- Decryption: `m = c^d % n`

The security of RSA comes from the fact that it is basically impossible to reverse engineer the private value of *d* from the public values of *n* and *e*. To do so requires factoring *p* and *q* from *n*, but that is extremely difficult to do with current technology, especially with very large values of *p* and *q*.

Now for the challenge itself. The values for *p*, *q*, *e*, and *d* are already given, leaving only the values of *n* and *$\Phi$(n)* to be calculated. Note that, in this scenario, *$\Phi$(n)* is irrelevant, since both *e* and *d* are provided.
-> Multiply *p* and *q* (233 * 103) to find the value of *n* (23999).

Next, examine the ciphertext: `23573 3126 15706 8302 1454 1323 9458 23263 19516 1323 3948 16186 10442 1323 17781 8302 23036 15276 1323 10442 1323 9458 18962 11308 4186 17781 19292`.

A quick check in CyberChef does not provide any suggestions on how to decode this string of numbers. So, given that the challenge title and description point out that RSA is used for byte-by-byte encryption, it seems reasonable to assume that each 4- or 5-digit number in the ciphertext is encrypted individually. For example, the first number (23573) can be plugged into the decryption formula as *c*, like so: `m = c^d % n = 23573^17825 % 23999 = 80`.
-> Using a powerful calculator or website like WolframAlpha, continue to decrypt the ciphertext following the example shown above. This should result in the string `80 97 119 115 123 99 114 85 110 99 72 121 95 99 48 115 109 49 99 95 99 114 89 112 116 48 125`.

This string might require another layer of RSA decryption, but a quick check in CyberChef first is always a good idea. In this case, it reveals that the string is encoded in decimal, which is a method of representing ASCII or Unicode characters as numbers.
-> Convert the string from decimal to reveal `Paws{crUncHy_c0sm1c_crYpt0}`.

This is the flag!