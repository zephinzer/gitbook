---
description: >-
  This section contains how-tos for doing things regarding Digital Ocean
  platform tools and infrastructure
---

# Use Digital Ocean

## Installing and configuring \`doctl\`

Refer to the [official guide](https://docs.digitalocean.com/reference/doctl/how-to/install/)

## Verifying that \`doctl\` is correctly configured

```bash
doctl account get
```

## Getting Kubernetes credentials

Retrieve the available Kubernetes clusters

```bash
doctl k8s cluster list
```

Identify the cluster of interest and copy it's ID, we refer to this as `${ID}` in the following command. Retrieve and save the connection configuration in your `~/.kube/config` file

```bash
doctl k8s cluster kubeconfig save ${ID}
```



