# Various 

## Dev server mode
- Initialized & unsealed
- In-mem storage
- Bound to local address without TLS
- Auto authenticated
- Single unseal key
- v2 KV mounted at `secret/`

## [Lease, Renew, and Revoke](https://developer.hashicorp.com/vault/docs/concepts/lease)
Lease: metadata like time duration, renewability... 

You can revoke with:
```sh
vault lease revoke
```

You can give a duration (in seconds) to the lease when renewing.

Revoke by prefix:
```sh
vault lease revoke -prefix <Prefix>
```

## [Response Wrapping](https://developer.hashicorp.com/vault/docs/concepts/response-wrapping)
Value is not the actual secret but a reference. If someone cannot unwrap token you could detect 
malfeasance. Limit the lifetime of secret exposure. Response will contain:
- TTL
- Token
- Creation: time the response-wrapping token was created
- Creatop Path
- Wrapped Accessor

Paths:
- `sys/wrapping/lookup`
- `sys/wrapping/unwrap`
- `sys/wrapping/rewrap`: migrate to a new response-wrapping token
- `sys/wrapping/wrap`

## [Username Templating](https://developer.hashicorp.com/vault/docs/concepts/username-templating)
Generated dynamic users for external systems.
- Functions: `lowercase`, `replace`, `trucate`, `truncate_sha256`, `uppercase`
- Generating Functions: `random`, `timestamp`, `unix_time`, `unix_time_millis`, `uuid`
- Hashing: `base64`, `sha256`

## Raft
Join by cmd:
```sh
vault operator raft join <LeaderAddress>
```

## Recovery mode
Use of a recovery token. 

Process:
- Seal/stop nodes
- If Integrated Storage, check the highest-index ones
- Restart target node in this mode
- Generate a recovery token
- Use it to perfom sys/raw requests to repair
- If Integrated St, reform it (data won't be the same)
    - Delete data on the other nodes and re-join them

## Transform Secrets Engine
Mechanims for tranforming sensitive info to protect it.
- **Format Preserving Encryption (FPE)**: encrypting & decrypting values while retaining their 
formats
- **Masking**: replacing sensitive info with masking characters
- **Tokenization**: replaces sensitive info with mathematically unrelated tokens

![Encryptions](encryptions.avif)

## [Mount migration](https://developer.hashicorp.com/vault/docs/concepts/mount-migration)
This is an async process `sys/remount`. To check the status `sys/remount/status`. To move across 
namespaces, the namespaces (src, dest) should be: from where the process is invoked or a children
of it. Leases, dynamic secrets (secrets mount) and tokens (auth mount) will be revoked. 
Configurations stay.

### Cleanup
Target entities & aliases to the new mount (if anyone pointed to the old one). Check if you need to
do some refactoring to the policies (if any targeting the old one).