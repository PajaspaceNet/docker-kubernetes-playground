

# 📈 Kubernetes – škálování

## 🔹 Manuální škálování
Pro změnu počtu instancí služby:
```bash
kubectl scale deployment <name> --replicas=3
````

Tímto nastavíš, že Deployment má běžet na 3 replikách (Podách).

---

## 🔹 Automatické škálování (HPA – Horizontal Pod Autoscaler)

Sleduje metriku (nejčastěji CPU nebo paměť, případně vlastní metriky přes Prometheus/Custom Metrics API).<br>
Když průměrné využití přesáhne nastavený práh, HPA zvýší počet podů.<br>
Když zatížení klesne, počet podů zase sníží.<br>
Pro nastavení automatického škálování podle CPU:<br>

```bash
kubectl autoscale deployment <name> --min=2 --max=10 --cpu-percent=80
```

* `--min` = minimální počet Podů
* `--max` = maximální počet Podů
* `--cpu-percent` = když průměrné CPU přesáhne hodnotu (např. 80 %), přidají se repliky
---
Jasně 👍, můžu ti připravit Markdown (`.md`) dokument, který bude jako malý návod s ukázkami.

Tady je návrh:

````markdown
# Kubernetes HPA – Příklad použití

## 1. Deployment aplikace

Nejdříve si vytvoříme jednoduchý `Deployment` s nastavenými resource requests/limits:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: myrepo/myapp:latest
          resources:
            requests:
              cpu: 100m
            limits:
              cpu: 500m
````

> ⚠️ Bez `resources.requests.cpu` nebude HPA fungovat.

---

## 2. Vytvoření HPA (příkazem)

HPA můžeš vytvořit jedním příkazem:

```bash
kubectl autoscale deployment myapp --cpu-percent=70 --min=2 --max=10
```

* `--cpu-percent=70` → cílové průměrné CPU využití 70 %
* `--min=2` → nikdy nesníží pod 2 pody
* `--max=10` → nikdy nepřekročí 10 podů

---

## 3. Vytvoření HPA (YAML)

Pokud chceš HPA spravovat přes GitOps nebo verzovat v Gitu, vytvoř si manifest:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

Aplikuj ho:

```bash
kubectl apply -f hpa.yaml
```

---

## 4. Kontrola HPA

Zkontroluj stav:

```bash
kubectl get hpa
```

Výstup bude vypadat např. takto:

```
NAME        REFERENCE          TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
myapp-hpa   Deployment/myapp   55%/70%    2         10        2          3m
```

---

## 📌 Shrnutí
* **Manuální škálování** = vhodné při testování nebo když víš, že potřebuješ přesně X instancí.
* **Automatické škálování (HPA)** = hodí se pro produkci, protože se počet Podů přizpůsobuje zátěži,přidává nebo ubírá pody podle zátěže (např. CPU)
* **Deployment** = spustí aplikaci s fixním počtem podů.
* Kombinací obou dosáhneš automatického horizontálního škálování.

---





