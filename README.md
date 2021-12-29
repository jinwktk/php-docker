# PHP Docker

```bash
// ref. https://www.engilaboo.com/apache-docker-https/
$ cd docker/php/ssl
$ openssl genrsa -aes128 2048 > server.key
$ openssl req -new -key server.key > server.csr
$ openssl x509 -in server.csr -days 365 -req -signkey server.key > server.crt
$ mv server.key server.key.org
$ openssl rsa -in server.key.org > server.key
```
