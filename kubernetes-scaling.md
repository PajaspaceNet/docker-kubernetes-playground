

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

## 📌 Shrnutí

* **Manuální škálování** = vhodné při testování nebo když víš, že potřebuješ přesně X instancí.
* **Automatické škálování (HPA)** = hodí se pro produkci, protože se počet Podů přizpůsobuje zátěži.

```


