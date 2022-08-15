# Intro
Secret management. Managing a set of diff credentials (authentication). Have more control & 
visibility over the use of secrets. Use of encryption to safeguard secret data. 
Features:
- **Rotation** of short lived, ephemeral credentials (dynamically created)

## Dev Mode
- Local **development, testing & exploration**
- Not secure
- In-memory
- Automatically unsealed
- Optionally set the initial root token

```sh
vault server -dev
```

To work around you must open a new terminal:
- Copy and export `VAULT_ADDR`

## Root token
You can perform any operation like `vault token create`.

You can revoke it with `vault tolen <Token>`.

## Secrets Engines
Store, generate or, encrypt secrets. Some can:
- Connect to other services & generate dynamic creds on demand
- Provide encryption as a service

Get a list of all enabled secrets engines:
```sh
vault secrets list
```

Enable engine:
```sh
vault secrets enable -path=<Path> <Type>
```

Disable engine:
```sh
vault secrets disable <Path>
```

Default engines:
```sh
sys/ #system backend
```

Get help about a secret engine (you must specify a root path, it will give an oveview of the engine):
```sh
vault path-help <Path>
```

### KV engine
An KV v2 secret engine is enabled in dev mode. The secrets are emcrypted before sending
them to the backend storage. The v1 of the KV does not support versioning.

Put values:
```sh
vault kv put <Path> <Key>=<Value>
```

Get values from a path:
```sh
vault kv get <Path>
```

Specific field:
```sh
vault kv get -field<Key> <Path>
```

## Dynamic secrets
Created when needed.
- AWS

### [AWS Engine](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-dynamic-secrets)
Add credentials to the `aws/config/root`.

Creates roles at `aws/roles`.

Add a role with 
```sh
vault write aws/roles/<NewRole>
``` 

Generate the secret:
```sh
vault read aws/creds/my-role
```

Revoke the secret:
```sh
vault lease revoke <LeaseId>
```

The default expiration time is 768 hours. 

## Auth
Enable:
```sh
vault auth enable github
```

See all enabled auth methods:
```sh
vault auth list
```

Help:
```sh
vault auth help <Path>
```

## [Policies](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-policies)
What a user can access (authorization). The most specific defined policy is used:
- Exact match
- Longest-prefix glob match

Control over:
- Enabling secrets engines
- Enabling auth methods
- Authenticating
- Secret access

Built-in policies that cannot be removed:
- `root`
- `default` (used by all tokens)

Read policy:
```sh
vault policy read <PolicyName>
```

Write policy:
```sh
vault policy write <PolicyName> <PathToFile> | <Stdin>
```

Policies are attached to tokens:
```sh
vault token create -policy=<PolicyName>
```

### Policies to Auth Methods
Depends. Typically mapping a role to policies or mapping identities/groups to policies.

## More production-like
Data folder must exit.
Start server:
```sh
vault server -config=<ConfigFile>
```

For maximum security your system must support `mlock`.

To start the unsealing process (new Vault cluster):
```sh
vault operator init
```

The `root token` and the `unseal keys` will appear. For increasing security, you can use some of 
the next [encryption methods](https://developer.hashicorp.com/vault/docs/concepts/pgp-gpg-keybase):
- Keybase
- PGP
- GPG

To unseal use:
```sh
vault operator unseal
```

Until you pass the threshold share.