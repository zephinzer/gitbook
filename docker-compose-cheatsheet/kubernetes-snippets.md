# ☸ Kubernetes Snippets

## Workload Volume Binding

```yaml
kind: Deployment
spec:
  template:
    spec:
      containers:
          volumeMounts:
          - name: __volume_name__
            mountPath: /path/to/directory
          - name: __volume_name__
            mountPath: /path/to/some/file.ext
            subPath: file.ext
          # ...
      volumes:
      - name: __volume_name__
        secret:
          defaultMode: 440
          secretName: {{ include "template.fullname" . }}-volume01
      # ...
    # ...
  # ...
# ...
```

## Workload Checksum

```yaml
kind: Deployment
spec:
  template:
    metadata:
      annotations:
        checksum/configmap: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}
        checksum/secrets: {{ include (print $.Template.BasePath "/secrets.yaml") . | sha256sum }}
      # ...
    # ...
  # ...
# ...
```
