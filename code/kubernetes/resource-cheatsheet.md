# Resource Cheatsheet

## CronJob

```yaml
# replace __application__ with your application name
# replace __docker__/__image__:__tag__ with your image
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: __application__
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      successfulJobsHistoryLimit: 3
      failedJobsHistoryLimit: 5
      template:
        spec:
          containers:
          - name: hello
            image: __docker__/__image__:__tag__
            imagePullPolicy: IfNotPresent
            args:
            - /bin/sh
            - -c
            - date; echo "Hello!"
          restartPolicy: OnFailure
```

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
        - name: http
          containerPort: 8000
          protocol: TCP
        resources:
          limits:
            memory: 25Mi
            cpu: 75m
          requests:
            memory: 20Mi
            cpu: 50m
```

## Job

```yaml
# replace __application__ with your job name
# replace __docker__/__image__:__tag__ with your image
apiVersion: batch/v1
kind: Job
metadata:
  name: __application__
spec:
  template:
    # This is the pod template
    spec:
      containers:
      - name: hello
        image: __docker__/__image__:__tag__ 
        command: ['sh', '-c', 'while :; do echo "Hello!"; sleep 5; done']
      restartPolicy: OnFailure
```

## Pod

```yaml
# replace __application__ with your job name
# replace __docker__/__image__:__tag__ with your image
apiVersion: v1
kind: Pod
metadata:
  name: __application__
  labels:
    app: __application__
spec:
  containers:
    - name: web
      image: __docker__/__image__:__tag__ 
      command: ['sh', '-c', 'echo "Hello!" && sleep 3600']
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
```

## Service

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
