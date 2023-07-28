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

## Istio AuthorizationPolicy to blocklist by hostname

```
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: {{ include "template.name" . }}-unity
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  selector:
    matchLabels:
      app.kubernetes.io/instance: {{ include "template.name" . }}
  action: DENY
  rules:
  - to:
    - operation:
        hosts:
          - hostname1.domain.com
          - hostname2.domain.com
```
