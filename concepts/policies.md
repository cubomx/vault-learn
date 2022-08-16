# [Policies](https://developer.hashicorp.com/vault/docs/concepts/policies)
Syntax:
```hcl
# This section grants all access on "secret/*". Further restrictions can be
# applied to this broad policy, as shown below.
path "secret/*" {
  capabilities = ["create", "read", "update", "patch", "delete", "list"]
}

# Even though we allowed secret/*, this line explicitly denies
# secret/super-secret. This takes precedence.
path "secret/super-secret" {
  capabilities = ["deny"]
}

# Policies can also specify allowed, disallowed, and required parameters. Here
# the key "secret/restricted" can only contain "foo" (any value) and "bar" (one
# of "zip" or "zap").
path "secret/restricted" {
  capabilities = ["create"]
  allowed_parameters = {
    "foo" = []
    "bar" = ["zip", "zap"]
  }
}
```

## Capabilities
- `create` (`POST/PUT`)
- `read` (`GET`)
- `update` (`POST/PUT`)
- `patch` (`PATCH`)
- `delete` (`DELETE`)
- `list` (`LIST`)

Not HTTP verbs:
- `sudo` (root)
- `deny`: high priority

## Precedence
1. If the first wildcard (`+`) or glob (`*`) occurs earlier in `P1`, `P1` is lower priority
2. If `P1` ends in `*` and `P2` doesn't, `P1` is lower priority
3. If `P1` has more `+` (wildcard) segments, `P1` is lower priority
4. If `P1` is shorter, it is lower priority
5. If `P1` is smaller lexicographically, it is lower priority