# Vault
Identity-based secrets & encryption management system. A **secret** is anything to tightly control
access to:
- API encryption keys
- Passwords
- Certificates

## How it works?
Tokens are associated with a client's policy. They are **path-based** and policy rules contrains 
the:
- Actions
- Accessibility

1. **Authenticate**: client supply identity. A token is generated when it passes and associated to
a policy
2. **Validation**: against 3rd-party trusted sources (GitHub, LDAP, AppRole)
3. **Authorize**: client matched against the Vault security policy.
4. **Access**: grant access to client using their Vault token. 

## Key features
- **Secure Secret Storage**: KV, Consul, more. Data is encrypted prior to writing them to persistent
storage.
- **Dynamic secrets**: on-demand. After creation and use, automatically revoked (lease is up).
- **Data Encryption**: encrypt/decrypt data withou storing it. External sources doesn't require
encryption methods.
- **Leasing & Renewal**: lease (ttl). At the end, revoked. It can be renewed.
- **Revocation**: of a tree of secrets (type, specific user). Key rolling, locking down systems.