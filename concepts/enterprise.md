# [Enterprise](https://developer.hashicorp.com/vault/docs/enterprise)

## Replication (ALL)
Consistency, scalability, and highly-available disaster recovery. A cluster (n active and its 
corresponding HA nodes). It also exists a **leader** (primary) cluster; it acts as the system of
record and it replicates asyn to others. 

Communication between clusters goes through mutual TLS sessions.

2 types:
- **Disaster Recovery**
    - Secondary cluster cannot handle client requests
- **Performance Replication** (Premium)
    - Keep track of their own tokens & leases

Both keep track of the:
- Configuration of the primary cluster
- Config of PC's backends

## HSM Support (Plus)
- Auto unsealing
- Root key wrapping: not split. Transit through HSM for encryption.

## PKCS#11 Provider (Vault 1.11+)
Access cryptographic capabilities on a device. Open Std C library API. Vault can be used as an 
SSM (Software Security Module). Connects to [KMIP secrets engine](https://developer.hashicorp.com/vault/docs/secrets/kmip).

## Automated Integrated Storage Snapshots
Taking regular backups. The active node performs it.
- Storage: `local` or `cloud` (AWS S3)

## Automated upgrades

## Redudancy zones
Scaling and resiliency by enabling the deployment of non-voting members nodes.

## Lease Count Quotas
Limit quantity of leases created. Could still create a root token. Batch tokens does not affect this
limit but, you won't be able to create them.

## Entropy Augmentation (Plus)
Randomness for cryptographic operations from external cryptographic modules via the `seals` 
inteface.

## Seal Wrap (Plus)
Wrap values with an extra layer of encryption for supporting seals.

## Namespaces
Isolated environments (tenants).

## Performance Standby Nodes (Premium)
Multi-server mode for HA. Enabled when using a data store that supports it.

## Control Groups
Add additional authorization factors required before satisfying a request. A response wrapping token
is returned instead of the data until the authorizers are certain. Then, this token can be used to
unwrap.

## MFA
- TOTP (passcode)
- OKTA (push notification)
- Duo (push notification)
- PingID (push notification)

## Sentinel (Plus)
Rich set of access control functionality.