# Použijeme Debian jako základ
FROM debian:stable

# Aktualizace balíčků a instalace asciiquarium
RUN apt-get update && \
    apt-get install -y asciiquarium && \
    rm -rf /var/lib/apt/lists/*

# Výchozí příkaz - spustí rovnou akvárium
CMD ["asciiquarium"]
