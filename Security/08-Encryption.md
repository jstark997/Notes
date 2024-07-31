# Encryption

A method for transforming comprehensible data (plain text) into incomprehensible data (cipher text) by use of a key and an encryption algorithm. The larger the key the harder it is to break the encryption but also the longer it takes to perform the encryption and decryption.

## Symmetric Encryption

Uses a single key to encrypt and decrypt. Often a password created by the user is converted into an encryption key used to encrypt the file(s). This password is then used later to decrypt the file(s) (again after being converted into an encryption key).

It is important to use a long and random password in order to ensure the encrypted data is very difficult to crack.

Symmetric encryption is used in most encryption systems, including HTTPS, file encryption, disk encryption, VPNs, Tor, etc.

The downside to symmetric encryption is that the user that needs to decrypt the cipher text needs the password. Could send via an out of band metod such as calling or text message, which is ok for one off occurances but is not scalable.

Advantages

- Fast
- Strong

Disadvantages

- Key (password) distribution is tricky
- Not scalable
- No authentication or non-repudiation

## Asymetric Encryption

Uses 2 keys - public and private.

If you encrypt with the private key you need the public key to decrypt.
If you encrypt with the public key you need the private key to decrypt.

For confidentiality, the sender can encrypt using the receiver's public key and then only the receiver can decrypt the message with their private key.

For authentication, the sender can encrypt with their private key and the receiver can decrypt with the sender's public key verifying that the message came from the sender.

Advantages

- Better key distribution (as compared to symmetric algorithms)
- Scalability
- Authentication and non-repudiation

Disadvantages

- Slow
- Mathematically intensive

## Hybrid Systems

Public and private keys are used to agree on and exchange keys, which are then used with symmetric algorithms to encrypt the data to be sent.

For example could use symmetric encryption to encrypt a message and send that to the receiver. Then use the receiver's public key to encrypt the password for the message and send that to the receiver. The receiver then uses their private key to decrypt the password, which they then use to decrypt the message.

This is what HTTPS using TLS or SSL does.

## Hash Functions

A hash function takes data of any size and returns a fixed length set of characters called the 'hash', 'message digest' or 'digest'.

A hash can not be converted back into the original data.

Uses

- Establishing integrity against unintentional modifications of the data.
- To encrypt passwords stored in a database.
- Pre-agreed shared secret in a message and then hash the message, HMAC (authenticaton and integrity).

## Digital Signatures

A digital signature is a hash value that has been encrypted with a sender's private key.

Provides authentication, integrity and non-repudiation.

To verify a digital signature:

1. Decrypt with the sender's public key, which reveals the hash value of the file that was signed
2. Take the hash algorithm used to create the hash value and hash the file that was sent.
3. Compare the hash value from the signature to the hash value created from the file.
4. If they match the file is legitimately from the sender.

## Secure Socket Layer (SSL) and Transport Layer Security (TLS)

Designed to provide communication security over a network.

SSL is older and less secure than TLS.

Both use symmetric and asymmetric encryption, as well as hashes, digital signatures, message authentication codes.

HTTPS uses TLS, but TLS can be used with other protocols such as FTP or VPNs.

TLS provides:

- Privacy from encryption
- Data integrity from message authentication codes (MAC)

TLS

- Generates a unique symmetric encryption key for each connection based on a secret negotiated at the start of the session.
- Encrypts the data sent using the generated key and symmetric algorithm
- Identity can be authenticated using public key cryptography certificates and digital signatures
- Each message sent includes a message integrity check using a MAC, to prevent undetected loss or alteration fo the data.

The combination of algorithms used is known as a _cipher suite_.

## Forward Secrecy

Assures that previous session keys (like those used in TLS) will not be compromised if the private key on the server is compromised. This requires that an algorithm like Diffie-Hellman is used to negotiate the session keys.

## SSL Stripping

A man in middle attack where the attacker acts as a proxy and converts the initial HTTPS connection into an insecure HTTP connection.

On a local network this could be accomplished through a technique called ARP spoofing.

A way to defeat this sort of attack is to tunnel to encrypt the traffic (using SSH or a VPN).

The server can use HTTP Strict Transport Security to tell the browser to only accept HTTPS traffic. Only works if the user has visited the site before.

## Server Name Indication (SNI)

An extension to TLS by which a client indicates which hostname it is attempting to connect to. Allows a server to present multiple certificates on the same IP address and port.

The problem is that the hostname is not encrypted so an eavesdropper will be able to see it.

## Digital Certificates

Used to authenticate public keys.

The most used standard is X.509. Unfortunately this is a weak standard.

Digital certificates typically contain information about the owner of the certificate.

The public key and the digital signature that authenticates the public key are validated by a trusted and authorized certificate authority.

Digital certificates depend on a chain of trust, where at the start of the chain is the root certificate authority which is trusted by operating systems and browsers (which contain a list of root certificates that they trust).

The problem with this system is that it depends on the reliablity and security of the certificate authority. Many certificate authorities have in error issued certifictates that were not requested by the domain owner. There are many certificate authorities and this means many opportunities for errors or security breaches. One way to mitigate this problem is the use a VPN (but that is only secure up to the VPN terminating point).

## End to End Encryption (E2EE)

Data is encrypted starting from where it is sent until it is received by the intended recipient. This is the most secure form of encryption.

## Steganography

Is the practice of concealing information or files in other non-secret text or data. For example, hiding a text file within an image file. Steganography by itself only hides the data it does not encrypt it. Although there are tools that can hide encrypted messages in a steganographic way.

The non-secret file is called the carrier. Images, audio and video files are some of the best and most common carriers.
