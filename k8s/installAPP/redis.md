# 部署redis



## 新建命名空间

```bash
kubectl create ns redis
```

## 新建配置文件

redis-config.yaml

```yaml

---
kind: ConfigMap
apiVersion: v1
metadata:
  name: redis-config
  namespace: redis
  labels:
    app: redis
data:
  redis.conf: |-
    dir /srv
    port 6379
    bind 0.0.0.0
    appendonly yes
    daemonize no
    #protected-mode no
    requirepass haojiayi
    pidfile /srv/redis-6379.pid


```

## 新建deployment

redis-deploy.yaml

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: redis
  labels:
    app: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:3.0.7
        command:
          - "sh"
          - "-c"
          - "redis-server /usr/local/redis/redis.conf"
        ports:
        - containerPort: 6379
        resources:
          limits:
            cpu: 1000m
            memory: 1024Mi
          requests:
            cpu: 1000m
            memory: 1024Mi
        livenessProbe:
          tcpSocket:
            port: 6379
          initialDelaySeconds: 300
          timeoutSeconds: 1
          periodSeconds: 10
          successThreshold: 1
          failureThreshold: 3
        readinessProbe:
          tcpSocket:
            port: 6379
          initialDelaySeconds: 5
          timeoutSeconds: 1
          periodSeconds: 10
          successThreshold: 1
          failureThreshold: 3
        volumeMounts:
        - name: config
          mountPath:  /usr/local/redis/redis.conf
          subPath: redis.conf
      volumes:
      - name: config
        configMap:
          name: redis-config

```



### 创建deployment

让config生效

```bash
kubectl apply -f redis-config.yaml  -n
```

创建delpoyment

```bash
kubectl apply -f redis-deploy.yaml -n redis  --force
```

创建过程

```bash
PS C:\Users\Troila\.kube\redis> kubectl apply -f redis-config.yaml  -n redis
configmap/redis-config created
PS C:\Users\Troila\.kube\redis> kubectl apply -f redis-deploy.yaml -n redis  --force
deployment.apps/redis created
```

查看pods状态

```bash
kubectl get pods -n redis
NAME                     READY   STATUS    RESTARTS   AGE
redis-55c57696d6-gtn8x   1/1     Running   0          62s
```

查看pod ip

```
kubectl get pods -n redis  -o wide
```

## 创建service

reids-svc.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis
  namespace: redis
  labels:
    app: redis
spec:
  type: NodePort
  ports:
    - name: tcp
      port: 6379
      nodePort: 36379
  selector:
    app: redis

```

应用yaml

```bash
kubectl apply -f redis-svc.yaml -n redis
```

