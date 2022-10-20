# Data Encryption
**Transit** secrets engine handles *cryptographic functions* on data in-transit.
- Sign & verify data
- Generate hashes & HMACs of data
- Act as a source of random bytes

It handles encryption/decryption of data.

## Key derivation
Use the same key for multiple purposes by:
- Deriving a *new key* based on a user-supplied context

**Convergent encryption** can be used in this mode and it allows the same input values to produce
the same ciphertext.

## Datakey generation
Allow processes to request a *high-entropy key* of a certain bit length.

## Manage
Encrypt:
```sh
vault write <TransitPath>/encrypt/<KeyRingName> plaintext=<Value>
```

Decrypt:
```sh
vault write <TransitPath>/encrypt/<KeyRingName> ciphertext=<Value>
```

Rotate key:
```sh
vault write -f <TransitPath>/keys/<KeyRingName>/rotate
```

Rewrap or modify ciphertext from a previous version of the key:
```sh
vault write <TransitPath>/rewrap/<KeyRingName> ciphertext=<Value>
```