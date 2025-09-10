# docker-playground-experiment
Toto repo se bude postupne rozsirovat , o jednotlive "vychytanky" o Dockeru

Repo obsahuje jednoduché playgroundy pro různé Linux OS.

Každá složka obsahuje:
- Dockerfile (může být prázdný, jen základní OS)
- run.sh – spustí interaktivní shell v příslušném OS

Spuštění:
```bash
sh <playground>/run.sh
```

Podporované OS:
- Debian Bullseye
- Ubuntu 22.04
- Fedora 38
- Alpine 3.18

docker build -t debian_playground ./debian_playground
docker build -t ubuntu_playground ./ubuntu_playground
docker build -t fedora_playground ./fedora_playground
docker build -t alpine_playground ./alpine_playground



docker run -it --rm debian_playground bash
