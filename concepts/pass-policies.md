# Password Policies (Vault 1.5+)
Set of instructions on how to generate a password. Used in a subset of secret engines (not all).
Randomly generated. Defined in HCL or JSON.

## Design
- Length
- Set of rules

## Things to consider
- A cryptographically secure random number generator (RNG)
- A character set.
- Length

We have a charset `abdcedfhij`. Then we choose the length (N). N numbers are generated 
(`[3, 2, 0, 8, 7, 3, 5, 1]`). A candidate pass is generated: `dcaihdfb`. Be careful: charset length
must be divisble into 256.


## Example
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
  charset = "!@#$%^&*"
  min-chars = 1
}
```
