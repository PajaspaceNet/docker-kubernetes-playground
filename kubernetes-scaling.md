

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

```
apiVersion: apps/v1              # Používáme API verzi pro Deployment (apps/v1 je stabilní)
kind: Deployment                 # Typ objektu = Deployment
metadata:
  name: myapp                    # Jméno Deploymentu (unikátní v namespacu)
spec:
  replicas: 2                    # Počet podů, které se mají spustit
  selector:                      # Jak Kubernetes pozná, které pody patří k tomuto Deploymentu
    matchLabels:
      app: myapp                 # Label, podle kterého se vybírají pody
  template:                      # Šablona, podle které se budou vytvářet pody
    metadata:
      labels:
        app: myapp               # Stejný label, aby seděl se selectorem
    spec:
      containers:
        - name: myapp            # Název kontejneru
          image: myrepo/myapp:latest   # Docker image aplikace
          resources:             # Omezení a požadavky na zdroje
            requests:            # Minimum, které kontejner potřebuje
              cpu: 100m          # 100 milicores (0.1 CPU)
            limits:              # Maximum, které kontejner může spotřebovat
              cpu: 500m          # 500 milicores (0.5 CPU)

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

apiVersion: autoscaling/v2       # Používáme API verzi pro HPA (v2 umožňuje více typů metrik)
kind: HorizontalPodAutoscaler    # Typ objektu = HPA
metadata:
  name: myapp-hpa                # Jméno HPA objektu (unikátní v namespacu)
spec:
  scaleTargetRef:                # Na co se má HPA vztahovat
    apiVersion: apps/v1          # API verze cílového objektu
    kind: Deployment             # Cíl je Deployment
    name: myapp                  # Název Deploymentu, který chceme škálovat
  minReplicas: 2                 # Minimální počet podů (nikdy nesníží pod 2)
  maxReplicas: 10                # Maximální počet podů (nikdy nepřekročí 10)
  metrics:                       # Definice metrik, podle kterých se škáluje
    - type: Resource             # Typ metriky = zdroje (CPU, paměť)
      resource:
        name: cpu                # Sledujeme CPU
        target:
          type: Utilization      # Typ cíle = průměrné využití (%)
          averageUtilization: 70 # Cíl = průměrné CPU využití 70 %

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





