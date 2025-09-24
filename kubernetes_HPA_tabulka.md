# HPA – příklad s výpočtem a reakcí
 **diagrame + konkrétní čísla podů**, prezentace  jak HPA reaguje:

## Zadání
Deployment má:
- `requests.cpu = 100m`
- HPA nastaveno na `averageUtilization: 70`
- `minReplicas: 2`
- `maxReplicas: 10`

---

## Situace 1 – start (2 pody)
Každý pod bere **150m CPU**.

Výpočet:
```

využití = 150m / 100m = 150 %

```

Průměrné využití podů:
```

(150 % + 150 %) / 2 = 150 %

```

ASCII schéma:
```

2 pody → 150 % > 70 % → SCALE UP

```

---

## Situace 2 – HPA přidá pody (4 pody)
Po navýšení na 4 pody se zátěž rozdělí.

Každý pod teď jede cca na **75m CPU**.

Výpočet:
```

využití = 75m / 100m = 75 %

```

Průměr:
```

(75 % + 75 % + 75 % + 75 %) / 4 = 75 %

```

ASCII schéma:
```

4 pody → 75 % ≈ 70 % → STABILNÍ

```

---

## Situace 3 – pokles zátěže (4 pody)
Celková zátěž systému klesne, každý pod jede jen na **30m CPU**.

Výpočet:
```

využití = 30m / 100m = 30 %

```

Průměr:
```

(30 % + 30 % + 30 % + 30 %) / 4 = 30 %

```

ASCII schéma:
```

4 pody → 30 % < 70 % → SCALE DOWN

```

---

## Situace 4 – po zmenšení (2 pody)
HPA sníží počet podů zpět na minimum (2).

Každý pod jede na **60m CPU**.

Výpočet:
```

využití = 60m / 100m = 60 %

```

Průměr:
```

(60 % + 60 %) / 2 = 60 %

```

ASCII schéma:
```

2 pody → 60 % < 70 %, ale blízko cíle → STABILNÍ

```

---

# Shrnutí

- HPA vždy porovnává **aktuální CPU / requests.cpu** a hledá průměr.  
- Když je průměr > 70 % → přidává pody (scale up).  
- Když je průměr < 70 % → ubírá pody (scale down).  
- Nikdy nesníží pod `minReplicas` a nepřekročí `maxReplicas`.  
