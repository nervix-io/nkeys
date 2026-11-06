[![Build Status](https://github.com/wasmCloud/nkeys/actions/workflows/build.yaml/badge.svg?branch=main)](https://github.com/wasmCloud/nkeys/actions/workflows/build.yaml)

# NKeys

Rust implementation of the [NATS nkeys](https://github.com/nats-io/nkeys) Go library.

The NATS ecosystem as of v2.x+ moved to Ed25519 keys for identity, authentication and authorization for entities such as Accounts, Users, Servers and Clusters.

Ed25519 is fast and resistant to side channel attacks. Generation of a seed key is all that is needed to be stored and kept safe, as the seed can generate both the public and private keys.

The NATS system will utilize Ed25519 keys, meaning that NATS systems will never store or even have access to any private keys. Authentication will utilize a random challenge response mechanism.

Dealing with 32 byte and 64 byte raw keys can be challenging. NKEYS is designed to formulate keys in a much friendlier fashion and references work done in cryptocurrencies, specifically Stellar. Bitcoin and others used a form of Base58 (or Base58Check) to endode raw keys. Stellar utilized a more traditonal Base32 with a CRC16 and a version or prefix byte. NKEYS utilizes a similar format where the prefix will be 1 byte for public and private keys and will be 2 bytes for seeds. The base32 encoding of these prefixes will yield friendly human readbable prefixes.

The following is a list of available prefixes and their keypair types:

* **N** - Server
* **C** - Cluster
* **O** - Operator
* **A** - Account
* **U** - User
* **M** - Module
* **V** - Service / Service Provider
* **P** - Private Key
* **X** - Curve Key (X25519)

For seeds, the first encoded prefix is **S**, and the second character will be the type for the public key, e.g. `SU` is a seed for a user key pair, `SA` is a seed for an account key pair.

## Format-only consumers

This maintained fork exposes `encode`, `encode_seed`, `decode_seed`, and `from_public_key`
without the default `signing` feature. Format-only consumers bring no signing, hashing,
key-generation or random backend into their dependency graph. Decoded seeds and intermediate
records clear their bytes on drop; public-key encoding allocates only its returned string.
The default signing and optional XKey APIs remain qualified alongside this surface.

The Nervix consumer pins an immutable revision and signs through its own cryptography owner.
Maintenance and qualification are tracked by [NATS Nervix 08](https://app.clickup.com/t/86bczq20q).
