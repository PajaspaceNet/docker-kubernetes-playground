


## 🔧 Spusteni `asciiquarium`
---
### Tohle je nase docker file ::

```yaml
# Použijeme Debian jako základ
FROM debian:stable

# Aktualizace balíčků a instalace asciiquarium
RUN apt-get update && \
    apt-get install -y asciiquarium && \
    rm -rf /var/lib/apt/lists/*

# Výchozí příkaz - spustí rovnou akvárium
CMD ["asciiquarium"]
```
---

## 🔨 Build image

Z terminálu v té složce spusť:

```bash
docker build -t asciiquarium_playground ./asciiquarium_playground
```

---

## 🐳 Spuštění

```bash
docker run -it --rm asciiquarium_playground
```

* `-it` → interaktivní mód, aby se to zobrazilo v tvém terminálu
* `--rm` → smaže container po ukončení (zůstane jen image)

---

💡 Tímhle způsobem si můžeš udělat „balíčky“ pro srandovní nástroje: `cowsay`, `cmatrix`, `sl`, `fortune`, prostě cokoliv.


