# Docker – základní příkazy

## 1. Informace o Dockeru
```bash
docker --version         # Zobrazí verzi Dockeru
docker info              # Zobrazí informace o Docker daemonu
```
## 2. Prace s Kontenjnery
 ```
docker ps                # Zobrazí běžící kontejnery
docker ps -a             # Zobrazí všechny kontejnery (i zastavené)
docker run <image>       # Spustí nový kontejner z image
docker run -it <image> /bin/bash   # Spustí kontejner interaktivně
docker stop <container_id>          # Zastaví běžící kontejner
docker start <container_id>         # Spustí zastavený kontejner
docker restart <container_id>       # Restartuje kontejner
docker rm <container_id>            # Odstraní kontejner
docker logs <container_id>          # Zobrazí logy kontejneru
docker exec -it <container_id> /bin/bash  # Připojí se do běžícího kontejneru
```
## 3.Práce s imigi
```
docker images            # Zobrazí dostupné image
docker pull <image>      # Stáhne image z Docker Hub
docker build -t <name>:<tag> .  # Vytvoří image z Dockerfile
docker rmi <image>       # Odstraní image
```
## 4. Docker Network
```
docker network ls                # Zobrazí seznam sítí
docker network create <name>      # Vytvoří novou síť
docker network inspect <name>     # Zobrazí detaily sítě
docker network rm <name>          # Odstraní síť
```
## 5. Docker Vomume
```
docker volume ls         # Zobrazí seznam volume
docker volume create <name>       # Vytvoří nové volume
docker volume inspect <name>      # Zobrazí detaily volume
docker volume rm <name>           # Odstraní volume
```
## 6. Dalsi uzitecne prikazy
```
docker system prune       # Odstraní nepoužívané kontejnery, image a sítě
docker stats              # Zobrazí využití CPU, RAM a I/O kontejnerů
docker-compose up         # Spustí služby definované v docker-compose.yml
docker-compose down       # Zastaví a odstraní kontejnery ze docker-compose.yml
```





