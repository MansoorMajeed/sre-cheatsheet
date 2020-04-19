# Kubernetes References

## Deployment

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: my-app
spec:
  template:
    metadata:
      labels:
        name: my-app
        app: my-appi
        env: production
        namespace: default
    spec:
      containers:
        - name: app
          image: node
          imagePullPolicy: Always
          resources:
              requests:
                  memory: "200Mi"
                  cpu: "200m"
              limits:
                  memory: "1000Mi"
                  cpu: "1"
          command:
              - npm
              - start
          ports:
            - containerPort: 3000
              name: node
              protocol: TCP
          envFrom:
          - configMapRef:
              name: my-app
          - secretRef:
              name: my-app
          env:
            - name: RESTART_
              value: "20181015015742"
            - name: DD_AGENT_SERVICE_HOST
              valueFrom:
                fieldRef:
                    fieldPath: status.hostIP
            - name: DD_AGENT_SERVICE_PORT
              value: "18125"
          readinessProbe:
            httpGet:
              path: /health_check
              port: node
            timeoutSeconds: 5
            periodSeconds: 10
            initialDelaySeconds: 30
          livenessProbe:
            httpGet:
              path: /health_check
              port: node
            timeoutSeconds: 5
            periodSeconds: 10
            initialDelaySeconds: 30
```

## Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-app
  labels:
    app: my-app
    env: production
    namespace: default
type: Opaque
data:
  PASSWORD: foo
```

## ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app
  labels:
    app: my-app
    env: production
    namespace: default
data:
  ENABLE_CONSOLE: '1'
  CONFIG: 'value'
```

## Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
  # Only for internal LB
  annotations:
    cloud.google.com/load-balancer-type: "Internal"
  labels:
    app: my-app
    env: production
    namespace: default
spec:
  type: LoadBalancer
  # To use static IP for LB
  loadBalancerIP: 10.142.0.32
  ports:
    - port: 3000
      targetPort: 3000
      protocol: TCP
  selector:
    name: my-app
    app: node-api
    env: blr
```

## HPA

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: my-app
  labels:
    application: my-app
    environment: production
    namespace: default
spec:
  scaleTargetRef:
    apiVersion: apps/v1beta1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```
