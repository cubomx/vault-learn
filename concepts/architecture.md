# Architecture
![Architecture](arch.avif)

Data in the storage backend is encrypted.
Barrier is the **encryption** layer.
*Requests* go to the **HTTP API**. When unsealed, those requests goes to the **Core**.
- The Core manages the:
    - Flow of requests 
    - Enforce ACLs (policies)
    - Ensure audit logging is done
    - Logs requests and responses to the audit broker (to all configured audit devices)
    - Lease management

## Vault & Consul
It is better to use Integrated Storage rather than Consul.
Rules:
- Consul should only be used for Vault purposes
- Use other ports: (8300 -> 7300, 8301 -> 7301)
