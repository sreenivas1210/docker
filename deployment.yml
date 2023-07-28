apiVersion: apps/v1
kind: Deployment
metadata:
  name: dotnet6-deployment
  namespace: sree
  labels:
    app: dotnet6
spec:
  replicas: 2
  selector:
    matchLabels:
      app: dotnet6
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 25%
      maxSurge: 25%
  template:
    metadata:
      labels:
        app: dotnet6
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: environment
                    operator: In
                    values:
                      - production
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: app
                    operator: In
                    values:
                      - dotnet6
              topologyKey: "kubernetes.io/hostname"
      containers:
        - name: dotnet6
          image: sreejags/dotnet:latest
          ports:
            - containerPort: 5000
          livenessProbe:
            httpGet:
              path: /_status/healthz
              port: 5000
            initialDelaySeconds: 8
            periodSeconds: 10
            failureThreshold: 3
            successThreshold: 1
          readinessProbe:
            httpGet:
              path: /readiness
              port: 5000
            initialDelaySeconds: 15
            periodSeconds: 5
          env:
            - name: DB_HOST
              valueFrom:
                configMapKeyRef:
                  name: dotnet6-deployment
                  key: DB_HOST