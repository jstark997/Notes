# Security Attributes

Security controls need to enable one or more security attributes.

## CIA Triad

- Confidentiality
- Integrity
- Availability

### Confidentiality

The asset is not disclosed to unauthorized persons. Its privacy is protected.

### Integrity

The asset is protected from being unintentionally altered. The asset cannot be modified in an unauthorized or undetermined way.

### Availability

The asset is always available when it is needed.

## Parkerian Hexad

- Confidentiality
- Possession: protect the assest from loss of control
- Integrity
- Authenticity: protect the veracity of the asset's origin claim or authorship
- Avaiability
- Utility: protect the assets continued usefulness

The CIA triad is an older model and some thought it did not cover enough attributes or speak well to the needs of businesses.

## Other Attributes

- Non-repudiation: protecting against false claims that an event actually did occur (a sender cannot deny sending a message that was actually received)
- Authentication: verifies the identity of a user or system
- Authorization: determines what the user or system is allowed to access and what it can do with those assets

## Requirements Gathering

In order to determine the proper security controls, requirements based on the security attributes an asset owner needs must be gathered.

## SABSA

SABSA (Sherwood Applied Business Security Architecture) is a list of security attributes designed to facilite communicating with businesses and gathering security requirements from them.

## Examples

- Encrypting an asset provides confidentiality
- Hashing an asset provides integrity
- Digitally signing an asset provides authentication, non-repudiation and integrity
- Encrypting and digitally signing an asset provides confidentiality, authentication, non-repudiation and integrity
