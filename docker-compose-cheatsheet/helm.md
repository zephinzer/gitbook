# ⛑ Helm

## Ensure service restarts on ConfigMap/Secret resource change

Add the following to your pod's annotation:

```yaml
kind: Deployment
# ...
spec:
  template:
    metadata:
      annotations:
        checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}
# ...
```
