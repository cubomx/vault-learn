# Secrets
They exist a place to manage the secrets: secrets engines. There exist some and not all perform the
same range of actions.

## Databases
Generates DB creds dynamically based on configured roles. It works through a plugin interface.

**Static roles** is a mapping 1-to-1 of Vault Roles to usernames in a DB.

You can enforce some rules for the password generation (**password policy**).

Default:
```hcl
length = 20

rule "charset" {
    charset = "abcdefghijklmnopqrstuvwxyz"
    min-chars = 1
}
rule "charset" {
    charset = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    min-chars = 1
}
rule "charset" {
    charset = "0123456789"
    min-chars = 1
}
rule "charset" {
    charset = "-"
    min-chars = 1
}
```

## KV
2 modes:
- Single (KV v1)
- Versioning (default: 10 versions)

## Identity
Internally maintains the clients who are recognized by Vault. Client == `Entity`. An **entity** can 
have many *Aliases*. The only credential backend that doesn't create a new entity is the *Token*. 
The capabilities granted to tokens via the entities are an addition to the existing capabilities of
the token and not a replacement. This is a default engine and cannot be removed.

## Manage
List all secret engines:
```sh
vault secrets list -detailed
```

Fully changed data at path with `vault kv put`. Just changing some part use `vault kv patch`.
If you want to get a version, use the `-version=<N>` flag.

Add metadata:
```sh
vault kv metadata put -custom-metadata=<Field>="<Value>" <Path>
```

Get metadata:
```sh
vault kv metadata get <Path>
```

Limit versions at engine:
```sh
vault write <PathEngine>/config max_versions=<N>
```

Limit by secret:
```sh
vault kv metadata put -max-versions=<N> <Path>
```

Delete versions of secret:
```sh
vault kv delete -versions="<V1>,<V2>" <Path>
```
Undelete with `vault kv undelete -versions=<N> <Path>`

Permanent delete:
```sh
vault kv destroy -versions=<N> <Path>
```

Automatic deletion:
```sh
vault kv metadata put -delete-version-after=<Time> <Path>
```

### Prevent overwrite
First, enable it:
```sh
vault write <EnginePath>/config cas_required=true
```

Or, by secret:
```sh
vault kv metadata put -cas-required=true <Path>
```

Use the `cas` flag with the current version:
```sh
vault kv put -cas=<CurrentN> <Path <Values>...
```

## Cubbyhole
A store for a person's token. Another user cannot access this kind of store from another person.
