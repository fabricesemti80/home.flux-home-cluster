# 🔒 OnePassword

- Install the [op cli](https://developer.1password.com/docs/cli/get-started/#install) following the instructions.
- [Connect to cli](https://developer.1password.com/docs/cli/get-started/)
- create secrets [guide](https://external-secrets.io/v0.5.7/provider-1password-automation/#deploy-a-connect-server)

```sh
## create vault
$ op vault create Kubernetes
# ID:                   gpuqypxjcegom3hypfbrstd744
# Name:                 Kubernetes
# Type:                 USER_CREATED
# Attribute version:    1
# Content version:      1
# Items:                0
# Created:              now
# Updated:              now

## create connect server
$ op connect server create Kubernetes --vaults Kubernetes
# File /home/fabrice/1password-credentials.json already exists, overwrite it? [Y/n] y
# Set up a Connect server.
# UUID: DN4A2BNR5BDQTB6UFS452RHSZ4
# Credentials file: /home/fabrice/1password-credentials.json

$  op connect token create Kubernetes --server Kubernetes --vaults Kubernetes
# < your token value>
```
