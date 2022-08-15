# Unseal & Seal

## [Seal/Unseal](https://developer.hashicorp.com/vault/docs/concepts/seal)
**Sealed**: Vault know where & how to access physical storage but *doesn't know how to decrypt*. 
Only possible operations: unseal and check the seal status.

Data is encrypted with the **encryption key** in the keyring. The keyring is encrypted by the
**root key**. The root key is encrypted by the **unseal key**.

How it gets sealed again?
- Server restarted
- Vault's storage layer encounters an unrecoverable error
- Resealed by API

### Auto Unseal
Keep the unseal key in a trusted device or service. At startup Vault will connect to this to 
implement the seal. Initializing with give `recovery keys` instead. They are generated from an 
internal recovery key (in the trusted entity) that is split via Shamir's Secret Sharing.

Flags (CLI):
- `recovery-shares`
- `recovery-threshold`
- `recovery-pgp-keys`: used to encrypt the returned recovery key shares.

### Rekey
Use:
```sh
vault operator rekey
```

Recovery:
```sh
vault operator rekey -target=recovery
```

Endpoints:
- `/sys/rekey`
- `/sys/rekey-recovery-key`

### Seal Migration
A downtime is going to exist.

Things to consider:
- Make a backup before
- Both old & new seals available
- Current limitation with AWSKMS to AWSKMS