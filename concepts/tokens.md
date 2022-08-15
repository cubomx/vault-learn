# Tokens
2 types:
- `service`
- `batch`

## Root token
Do anything. The only one to be set to never expire without any renewal.
3 ways to generate it:
- `vault operator init`
- By using another root token (a expiring token cannot create a non-expiring)
- `vault operator generate-root` with the permission of a *quorum of unseal key holders*

## Hierarchies & Orphans
If a parent is revoked, all child are revoked. To create a orphan token:
- `auth/token/create-orphan` endpoint
- `sudo` or `root` access to the `auth/token/create` & set the `no_parent` to `true`
- Logging in with any other non-token auth method
- Via **token store roles** (backend)

## Token Accessors
Limited to do the following:
- Look up a token's properties
- Look up a token's capabilities on a path
- Renew and revoke the token

View all tokens by accessors:
```sh
auth/token/accessors
```

## TTL, max TTL
Max TTL it may be:
- Default: 32 days
- Configuration file
- Set on a mount, using `mount tuning`
- Value suggested by an auth method

## Periodic tokens
- Sudo or root token in the `auth/token/create` endpoint
- Token store roles
- Auth method (like AppRole)

