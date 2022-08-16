# [Identity](https://developer.hashicorp.com/vault/docs/concepts/identity)
Maintain the clients who are recognized by Vault.

## Entities & Aliases
**Entity** is a representation of a consolidaded identity. **Aliases** are the corresponding
accounts with authentication providers (1 per particular authentication backend, mount different).
Vault will be a *cache of identities*.

## Entity Policies
If using a token from a entity, it will grant additional permissions on top of the existing 
policies. Name of policies will remain inmutables but the evaluation of policies will happen at
request time.

## Mount Bound Aliases
| Auth method | Name reported |
| ----------- | ------------- |
| AliCloud | Principal ID |
| AppRole | Role ID |
| AWS IAM | Role ID, IAM unique ID, Full ARN |
| AWS EC2 | Role ID, EC2 instance ID, AMI ID | 
| Azure | Subject (JWT claim) |
| GitHub | User login name |
| GCP | Role ID, Service account unique ID |
| K8s | SA ID |


## Identity Groups
Members inherit groups policies. 