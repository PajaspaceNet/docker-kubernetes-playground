# docker-playground-experiment
Toto repo se bude postupne rozsirovat , o jednotlive "vychytanky" o Dockeru

Repo obsahuje jednoduché playgroundy pro různé Linux OS.

## Každá složka obsahuje:
- Dockerfile (může být prázdný, jen základní OS)
- run.sh – spustí interaktivní shell v příslušném OS

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

  ### MAZANI ###
```
docker ps   # vylistuj contejnery 

docker stop fedora # STOP bud podle jmena stopni , nebo podle cisla
docker rm fedora  # smaz bud podle jmena stopni , nebo podle cisla
```

<br>
---- Jinak receno ----<br>
Image = předloha (neměnná)<br>
Container = běžící „kopie“ image, kde můžeš experimentovat<br>
Pokud container ukončíš bez --rm, změny zůstanou uvnitř containeru.<br>
Můžeš mít jednu image a více současně běžících containerů z ní<br>.



```
docker ps   # vylistuj contejnery 

docker stop fedora # STOP bud podle jmena stopni , nebo podle cisla
docker rm fedora  # smaz bud podle jmena stopni , nebo podle cisla
```
