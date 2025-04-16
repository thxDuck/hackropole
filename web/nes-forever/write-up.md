# Nes Forever

Exploitez une injection SQL afin de vous connecter sur l’application web.

## Description

First, download docker-compose.yml:

```bash
curl https://hackropole.fr/challenges/fcsc2021-web-push-it-to-the-limit/docker-compose.public.yml -o docker-compose.yml
```

Launch the challenge by executing in the same folder:

```bash
docker compose up
```

Access the challenge at <http://localhost:8000/>.

## Findings

1. Visiting <http://localhost:8000/>, this is a login page
2. Set random username and password display the information "Identifiants incorrectes".
3. If there are a double quote `"` in the username, we have a SQLite error

## Vulnerabilities

The username field contains a SQL injection.

## Exploitation

Whith a raw sql query whithout sanitization of input, and concatenation to create the query, we can control all the query.
Escape the query param with a quote `"` ans control the rest of the query :
Send the payload in the username field, with Url encoding: `admin1";--` (ecoded version: `admin1%22%3b--`)

```sql
SELECT * FROM users WHERE username="admin" AND password="pass" 
-- Become
SELECT * FROM users WHERE username="admin"--" AND password="pass" 
```

We have removed the password condition

## Remediation

Use prepared sql statement to prevent from alteration of original request.
Sanitize user input, remove all special and dangerous chars from user inputs.
