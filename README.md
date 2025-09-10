# docker-playground-experiment
Toto repo se bude postupne rozsirovat , o jednotlive "vychytanky" o Dockeru

Repo obsahuje jednoduché playgroundy pro různé Linux OS.

## Každá složka obsahuje:
- Dockerfile ( zakladni OS  , nebo nejakou vychytavku napr. asciiaquarium)
- run.sh – spustí interaktivní shell v příslušném OS
  *  nebo navod jak  spustit dany image

## Spuštění obecne:
```bash
sh <playground>/run.sh
```

Podporované OS:
- Debian Bullseye
- Ubuntu 22.04
- Fedora 38
- Alpine 3.18

### Buildovani a spousteni
* buildovat bud z Dockerfile a spustit z vlastniho image - musi by spravne nastavena cesta
```
docker build -t debian_playground ./debian_playground
docker run -it --rm debian_playground bash

docker build -t ubuntu_playground ./ubuntu_playground
docker run -it --rm ubuntu_playground bash

docker build -t fedora_playground ./fedora_playground
docker run -it --rm fedora_playground bash

docker build -t alpine_playground ./alpine_playground
docker run -it --rm alpine_playground bash
```
A nebo primo image 

```
docker run -it --rm debian:bullseye bash  - spusteni debianu 
docker run -it --rm ubuntu:22.04 bash    - spusteni ubuntu 
docker run -it --rm alpine:3.18 sh       -alpine linux
docker run  -it --rm fedora:38 bash  -fedora
```

### Obecne plati
```
-it → interaktivní terminál<br>
--rm → po ukončení se container smaže<br>
fedora:38 → image Fedora 38<br>
bash → spustí shell uvnitř containeru<br>
```
Pokud ale chceme na to tom dale pracovat , **aby se to nesmazalo co jsme instalovali**,  spustime Image spustime takto 
```
docker run  -it fedora:38 bash 
```
bez **--rm**
<br><br>
### pamatuj si...
* Container = běžící instance image, změny uvnitř zůstávají.<br>
* Image = samotný stav OS, který můžeš znovu spouštět.<br>
* Smazání probíhá jen explicitně, jinak všechno zůstává.<br>
* Takže všechno, co jsi **buildnul a nainstaloval, se neztratí, dokud to nesmažeš ty.** <br>

<br>
---- Jinak receno ----<br>
Image = předloha (neměnná)<br>
Container = běžící „kopie“ image, kde můžeš experimentovat<br>
Pokud container ukončíš bez --rm, změny zůstanou uvnitř containeru.<br>
Můžeš mít jednu image a více současně běžících containerů z ní<br>.

---

### MAZANI ###
```
docker ps   # vylistuj contejnery 

docker stop <container> # STOP bud podle jmena stopni , nebo podle cisla
docker rm <container>  # smaz bud podle jmena stopni , nebo podle cisla
```

**RESTART**<br>
stejneho kontejneru kdyz se to nepovede a chces to uplne znova 
```
docker ps   # vylistuj contejnery 

* docker stop <container> → zastaví běžící container
* docker rm <container> → smaže container
* docker build -t <image> <folder> → rebuild image
* docker run -it <image> bash → spustíš nový container
```
