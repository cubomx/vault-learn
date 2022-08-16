# [High Availability Mode (HA)](https://developer.hashicorp.com/vault/docs/concepts/ha)
Running multi-server (if data store supports it). One server grabs the lock and becomes the active
node. So, not really scalability. For that, you should implement 2 different storages: one for 
managing the lock (e.g. Consul) and another for the persisted data (Amazon S3).

## Server to Server Communication
- Client redirection: no direct
- Request forwarding: mTLS 1.2 connection between the servers. Active node puts in the encrypted 
data store a newly-generated private key ?(ECDSA-P521) and a newly-generated self-signed 
certificate. Each standby node use this.

`api_addr` or `VAULT_API_ADDR` should be set to the active node in a full URL schemd (`http` /
`https`).

Only active node have active listeners. In the block address you can put the `cluster_address` to 
perform server-to-server cluster requests.

## Supported storages
- Consul
- ZooKeeper
- etcd
- Vault integrated (raft)