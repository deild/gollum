Gollum
------
[Gollum] webapp on Debian 8 (Jessie) with support for strict HTTPS ([HSTS]).

For usage info, just run the image without a command:

```text
$ docker run --rm genebarker/gollum
```

Which produces the following:

```text
gollum - a Gollum webapp on Debian 8 Docker Container

usage: genebarker/gollum [OPTION]

The available OPTIONs are:
   --http [GOLLUMOPTION]...       Run Gollum using plain HTTP
   --hsts FQDN [GOLLUMOPTION]...  Run Gollum using HTTPS only
          (must provide FQDN, i.e. mybox.example.com)
   --help                         Display this message

To use wiki repository on the host, mount it, i.e.:
   $ docker run -d -p 80:80 \
       -v /home/me/wiki:/root/wiki \
       genebarker/gollum --http

To run with strict HTTPS creating new self-signed keys:
   $ docker run -d -p 80:80 -p 443:443 \
       genebarker/gollum --hsts mybox.example.com

To run with strict HTTPS using your own keys, mount them, i.e.:
   $ docker run -d -p 80:80 -p 443:443 \
       -v /etc/ssl:/etc/ssl \
       genebarker/gollum --hsts mybox.example.com

   (the cert's CN must match the FQDN)

To run as a rack application, place your config file in the repo,
mount it, and set the RACK_APP environment variable to its name:
   $ docker run -d -p 80:80 \
       -v /home/me/wiki:/root/wiki \
       -e RACK_APP=config.ru \
       genebarker/gollum --http

To bypass script, just enter desired command, i.e.:
   $ docker run -it genebarker/gollum bash

Key paths in the container:
   /root/wiki     - Wiki content (a git repository)
   /etc/ssl       - SSL keys and certificates
   /etc/ssl/private/ssl-cert-snakeoil.key  - Private SSL key
   /etc/ssl/certs/ssl-cert-snakeoil.pem    - Public SSL cert
```

### Notes ###

- This image uses GitHub Flavored Markdown ([GFM]).
- For a rack application, see the example `config.ru`, and be sure to append its required packages and gems to the respective RUN commands in the `Dockerfile`.
- To use the timezone settings of the host, mount the `localtime` and `timezone` files, i.e. on Debian hosts:
    `-v /etc/localtime:/etc/localtime -v /etc/timezone:/etc/timezone`

### Quick Start ###

(1) Create a Git repository for your wiki:

```text
$ cd
$ mkdir mywiki
$ cd mywiki
$ git init
```

(2) Add a starter document the repository *(optional)*:

```text
$ echo "Hello World!" > Home.md
$ git add Home.md
$ git commit -m 'Initial commit'
```

(3) Spin-up the Gollum container:

```text
$ docker pull genebarker/gollum
$ docker run -d -p 80:80 -v ~/mywiki:/root/wiki genebarker/gollum --http
```

[GFM]:https://help.github.com/articles/github-flavored-markdown/
[Gollum]:https://github.com/gollum/gollum
[HSTS]:http://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security
