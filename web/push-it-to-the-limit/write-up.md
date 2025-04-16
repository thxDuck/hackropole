# Nes Forever

## Description

First, download docker-compose.yml:

```bash
curl https://hackropole.fr/challenges/fcsc2020-web-nes-forever/docker-compose.public.yml -o docker-compose.yml
```

Launch the challenge by executing in the same folder:

```bash
docker compose up
```

Access the challenge at <http://localhost:8000/>.

## Findings

1. Visiting <http://localhost:8000/>

## Vulnerabilities

The html source contains the flag in comment.
