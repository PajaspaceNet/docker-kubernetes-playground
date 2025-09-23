

# 🚀 Kubernetes základy

Tento dokument shrnuje nejdůležitější pojmy a příklady z Kubernetes

---

## 🔹 Základní pojmy

1. **Pod**
   - Nejmenší jednotka v Kubernetes.
   - Obsahuje 1 nebo více kontejnerů, které běží společně (většinou 1 kontejner = 1 Pod).

2. **Deployment**
   - Deklaruje, kolik Podů má běžet a jak se mají spravovat.
   - Kubernetes zajišťuje, aby běžel vždy správný počet instancí.

3. **Service**
   - Poskytuje stabilní přístup k Podům.
   - Zajišťuje **load balancing** mezi více instancemi.

4. **Ingress**
   - Definuje pravidla, jak se dostaneš k aplikaci zvenčí (např. přes doménu a HTTPS).

5. **ConfigMap / Secret**
   - `ConfigMap` – ukládá konfigurační hodnoty (např. environment variables).
   - `Secret` – ukládá citlivá data (hesla, API klíče).

---

## 🔹 Architektura (ASCII diagram)

```txt
               +-------------------+
               |     Ingress       |
               |  (externí vstup)  |
               +---------+---------+
                         |
                         v
                 +-------+-------+
                 |    Service    |
                 | (LoadBalancer)|
                 +-------+-------+
                         |
             -------------------------
             |                       |
             v                       v
       +-----------+           +-----------+
       |   Pod     |           |   Pod     |
       | Container |           | Container |
       +-----------+           +-----------+
             |                       |
             v                       v
     +---------------+        +---------------+
     |   ConfigMap   |        |    Secret     |
     |  (nastavení)  |        | (hesla, klíče)|
     +---------------+        +---------------+
````

---

## 🔹 Základní příkazy (kubectl)

```bash
# seznam podů
kubectl get pods

# seznam služeb
kubectl get services

# nasazení z YAML manifestu
kubectl apply -f deployment.yaml

# smazání z YAML manifestu
kubectl delete -f deployment.yaml

# logy z konkrétního Podu
kubectl logs <pod-name>
```

---

## 🔹 Ukázkové manifesty

### 1. Deployment (spustí 2 instance Nginx)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

### 2. Service (zpřístupní Pody na portu 80 uvnitř clusteru)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-service
spec:
  selector:
    app: hello
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

### 3. Ingress (přístup zvenčí přes doménu `hello.local`)

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-ingress
spec:
  rules:
  - host: hello.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hello-service
            port:
              number: 80
```

### 4. ConfigMap (ukládá konfigurační hodnoty)

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  APP_DEBUG: "false"
  APP_VERSION: "1.0.0"
```

Použití v Podu:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

### 5. Secret (ukládá citlivá data)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_USER: dXNlcg==        # base64("user")
  DB_PASSWORD: cGFzc3dvcmQ= # base64("password")
```

Použití v Podu:

```yaml
envFrom:
  - secretRef:
      name: db-secret
```

---

## 📌 Shrnutí

* Kubernetes je **orchestrátor kontejnerů** – stará se o nasazování, škálování a dostupnost.
* **Pod** = základní jednotka.
* **Deployment** = správa počtu Podů.
* **Service** = stabilní přístup + load balancing.
* **Ingress** = vstup do clusteru zvenčí.
* **ConfigMap/Secret** = konfigurace a citlivá data.


```


