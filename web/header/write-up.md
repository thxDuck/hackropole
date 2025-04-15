# Babel web

## Description

First, download docker-compose.yml:

```bash
 curl https://hackropole.fr/challenges/fcsc2022-web-header/docker-compose.public.yml -o docker-compose.yml
```

Launch the challenge by executing in the same folder:

```bash
docker compose up

```

Access the challenge at <http://localhost:8000/>.

## Findings

1. Visiting <http://localhost:8000/source> get the source-code of Nodejs router.

## Vulnerabilities

The GET / route check if the `X-FCSC-2022` header is "Can I get a flag, please?", then, return fag.txt content in the page.

## Explotation

```bash
curl 'http://localhost:8000/' -H 'X-FCSC-2022: Can I get a flag, please?'
```

## Remediations
