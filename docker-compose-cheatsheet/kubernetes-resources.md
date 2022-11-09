---
description: >-
  This page provides template Kubernetes resources that can be copied and pasted
  into YAML files to be applied to a Kubernetes cluster using `kubectl apply -f
  ...`
---

# ☸️ Kubernetes Resources

## ClusterRole

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
rules:
  - apiGroups: [""]
    resources:
      - configmaps
      - endpoints
      - namespaces
      - nodes
      - pods
      - pods/logs
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: &readOnly
      - get
      - watch
      - list
  - apiGroups: [""]
    resources:
      - secrets
    verbs: &listOnly
      - list
  - apiGroups: ["apps"]
    resources:
      - controllerrevisions
      - deployments
      - daemonsets
      - replicasets
      - statefulsets
    verbs: *readOnly
  - apiGroups: ["autoscaling"]
    resources:
      - autoscaling
    verbs: *readOnly
  - apiGroups: ["batch"]
    resources:
      - cronjobs
      - jobs
    verbs: *readOnly
  - apiGroups: ["networking.k8s.io"]
    resources:
      - ingresses
    verbs: *readOnly
  - apiGroups: ["policy"]
    resources:
      - podsecuritypolicies
    verbs: *readOnly
```

## ClusterRoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
subjects:
- kind: ServiceAccount
  name: {{ .Values.serviceAccount.name }}
  namespace: {{ .Values.serviceAccount.namespace }}
roleRef:
  kind: ClusterRole
  name: {{ .Values.clusterRole.name }}
  apiGroup: rbac.authorization.k8s.io

```

## ConfigMap

```yaml
# replace __application__ with your application name
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
data:
  var1: value1
  var2: value2
```

## CronJob

```yaml
# replace __application__ with your application name
# replace __docker__/__image__:__tag__ with your image
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      successfulJobsHistoryLimit: 3
      failedJobsHistoryLimit: 5
      template:
        spec:
          containers:
          - name: {{ include "template.name" . }}
            image: "{{ .Values.image.repository }}:{{ required "The image.tag must be specified to deploy this" .Values.image.tag }}"
            imagePullPolicy: IfNotPresent
            args:
            - /bin/sh
            - -c
            - date; echo "Hello!"
          restartPolicy: OnFailure
```

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  replicas: 1
  selector:
    matchLabels:
      {{- include "template.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      labels:
        {{- include "template.labels" . | nindent 8 }}
    spec:
      containers:
      - name: {{ include "template.name" . }}
        image: "{{ .Values.image.repository }}:{{ required "The image.tag must be specified to deploy this" .Values.image.tag }}"
        imagePullPolicy: Never
        ports:
        - name: http
          containerPort: {{ .Values.service.port }}
          protocol: TCP
        envFrom:
        - secretRef:
            name: {{ include "template.fullname" . }}-env
            optional: false
        - configMapRef:
            name: {{ include "template.fullname" . }}-env
            optional: false
        resources:
          limits:
            memory: 25Mi
            cpu: 75m
          requests:
            memory: 20Mi
            cpu: 50m
        volumeMounts:
        - name: dir-mount
          mountPath: /path/to/dir/
        - name: file-mount
          mountPath: /path/to/file.ext
          subPath: file.ext
      volumes:
      - name: dir-mount
        secret:
          defaultMode: 440
          secretName: {{ include "template.fullname" . }}-dir
      - name: file-mount
        secret:
          defaultMode: 440
          secretName: {{ include "template.fullname" . }}-file
```

## Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 3
  template:
    # This is the pod template
    spec:
      containers:
      - name: hello
        image: "{{ .Values.image.repository }}:{{ required "The image.tag must be specified to deploy this" .Values.image.tag }}"
        command: ['sh', '-c', 'while :; do echo "Hello!"; sleep 5; done']
        envFrom:
          - secretRef:
              name: {{ include "template.fullname" . }}-env
              optional: false
          - configMapRef:
              name: {{ include "template.fullname" . }}-env
              optional: false
      restartPolicy: OnFailure
```

## Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  {{- if .Values.ingress.tls }}
  tls:
    {{- range .Values.ingress.tls }}
    - hosts:
        {{- range .hosts }}
        - {{ . | quote }}
        {{- end }}
      secretName: {{ .secretName }}
    {{- end }}
  {{- end }}
  rules:
    {{- range .Values.ingress.hosts }}
    - host: {{ .host | quote }}
      http:
        paths:
          {{- range .paths }}
          - path: {{ .path }}
            pathType: Prefix
            backend:
              service:
                name: {{ .serviceName }}
                port:
                  number: {{ .servicePort }}
          {{- end }}
    {{- end }}
```

## Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  containers:
    - name: web
      image: "{{ .Values.image.repository }}:{{ required "The image.tag must be specified to deploy this" .Values.image.tag }}"
      command: ['sh', '-c', 'echo "Hello!" && sleep 3600']
      ports:
        - name: web
          containerPort: {{ .Values.service.port }}
          protocol: TCP
```

## PodMonitor

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PodMonitor
metadata:
  name: {{ include "template.fullname" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  selector:
    matchLabels:
      {{- include "template.selectorLabels" . | nindent 6 }}
  podMetricsEndpoints:
  - port: http
```

## Role

```yaml
# replace __role__ with your role name
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
rules:
  - apiGroups: [""]
    resources:
      - configmaps
      - endpoints
      - namespaces
      - nodes
      - pods
      - pods/logs
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: &readOnly
      - get
      - watch
      - list
  - apiGroups: [""]
    resources:
      - secrets
    verbs: &listOnly
      - list
  - apiGroups: ["apps"]
    resources:
      - controllerrevisions
      - deployments
      - daemonsets
      - replicasets
      - statefulsets
    verbs: *readOnly
  - apiGroups: ["autoscaling"]
    resources:
      - autoscaling
    verbs: *readOnly
  - apiGroups: ["batch"]
    resources:
      - cronjobs
      - jobs
    verbs: *readOnly
  - apiGroups: ["networking.k8s.io"]
    resources:
      - ingresses
    verbs: *readOnly
  - apiGroups: ["policy"]
    resources:
      - podsecuritypolicies
    verbs: *readOnly
```

## RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
subjects:
  - kind: ServiceAccount
    name: {{ .Values.serviceAccount.name }}
    namespace: {{ .Release.Namespace }}
roleRef:
  kind: Role
  name: {{ .Values.role.name }}
  namespace: default
  apiGroup: rbac.authorization.k8s.io
```

## Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
type: Opaque
# either this ...
data:
  var1: d293IHlvdSBhY3R1YWxseSBkZWNvZGVkIHRoaXM=
  var2: YSBjdXJpb3VzIG9uZSwgeW91IGFyZQ==
# ... or this ...
stringData:
  var1: hello world
  var2: "12345"
```

## Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  selector:
    {{- include "template.labels" . | nindent 4 }}
  ports:
    - protocol: TCP
      port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.port }}
```

## ServiceAccount

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
```

## ServiceMonitor

```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: {{ include "template.fullname" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
spec:
  endpoints:
  - interval: 5s
    port: http
  selector:
    matchLabels:
      {{- include "template.selectorLabels" . | nindent 6 }}
```

## VirtualService

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: {{ include "template.name" . }}
  labels:
    {{- include "template.labels" . | nindent 4 }}
  annotations:
    external-dns.alpha.kubernetes.io/target: "{{ .Values.loadBalancerUrl }}"
spec:
  hosts:
    {{ .Values.hostUrls | toYaml | nindent 4 }}
  gateways:
    - istio-system/istio-ingress
  tcp:
  - match:
    - port: {{ .Values.service.port }}
    route:
    - destination:
        host: {{ include "template.name" . }}
        port:
          number: {{ .Values.service.port }}
```
