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