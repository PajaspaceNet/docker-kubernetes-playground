
---

## 🔧 Spusteni `asciiquarium`



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


