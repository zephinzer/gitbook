# Resource Cheatsheet

## Deployment

```yaml
# replace __application__ with your application name
# replace __docker__/__image__:__tag__ with your image
apiVersion: apps/v1
kind: Deployment
metadata:
  name: __application__
  labels:
    app: __application__
spec:
  replicas: 1
  selector:
    matchLabels:
      app: __application__
  template:
    metadata:
      labels:
        app: __application__
    spec:
      containers:
      - name: __application__
        image: __docker__/__path__:__tag__
        imagePullPolicy: Never
        ports:
        - containerPort: 8000
        resources:
          limits:
            memory: 25Mi
            cpu: 75m
          requests:
            memory: 20Mi
            cpu: 50m
```

### Service

```yaml
# replace __application__ with your service name
apiVersion: v1
kind: Service
metadata:
  name: __application__
spec:
  selector:
    app: __application__
  ports:
    - protocol: TCP
      port: 8000
      targetPort: 8000
```

