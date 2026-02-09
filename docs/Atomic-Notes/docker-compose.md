---
tags:
    - Arch
    - Docker
    - Docker-Compose
---

# Docker Getting Started & Compose Files

Docker compose is an easy and fast way to deploy application on the spot. The following `docker-compose.yml` are for future reference. Its options might defer from time to time, thus visit all apps oficial repos to get most-up-to-date information

```docker
version: "3.3"
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    ports:
      - 3000:3000
    env_file: .env # use .env
    volumes:
      - /home/username/apps/homepage:/app/config # Make sure your local config directory exists
      - /var/run/docker.sock:/var/run/docker.sock # (optional) For docker integrations, see alternative methods
    environment:
      PUID: $PUID # read them from .env
      PGID: $PGID # read them from .env
      HOMEPAGE_ALLOWED_HOSTS: localhost_ip:3000,local_dns_entry:3000
    restart: always
```

Create an `.env` file to store `PUID/PGID`
