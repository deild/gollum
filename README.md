## Gollum

> Forked & Inspired by from [Eugene F. Barker's gollum][6]

[Gollum][1] webapp on Debian 10 (Bullseye) with support for HTTP and strict HTTPS ([HSTS][2]).

### Improvements

This production proven image of Gollum just got a little better.. Enjoy!

- upgraded OS to Debian 10.3
- upgraded Gollum to 4.1.4
- remove Github Flavored Markdown (NOT MAINTAINED AND NOT SUPPORTED)
- add Asciidoctor 2.0.10
- improved this readme

## Quick Start

1) Create a Git repository for your wiki:

```text
$ cd
$ mkdir mywiki
$ cd mywiki
$ git init
```

(2) Add a starter document to the repository *(optional)*:

```text
$ echo "Hello World!" > Home.md
$ git add Home.md
$ git commit -m 'Initial commit'
```

(3) Spin-up the Gollum container:

```text
$ docker pull deild/gollum
$ docker run -d -p 80:80 -v ~/mywiki:/root/wiki deild/gollum --http
```

(4) Enjoy

## How to Use

For usage info, just run the image without a command:

```text
$ docker run --rm deild/gollum
```

Which produces the following:

```text
gollum - a Gollum webapp on Debian 10 Docker Container

usage: deild/gollum [OPTION]

The available OPTIONs are:
   --http [GOLLUMOPTION]...       Run Gollum using plain HTTP
   --hsts FQDN [GOLLUMOPTION]...  Run Gollum using HTTPS only
          (must provide FQDN, i.e. mybox.example.com)
   --help                         Display this message

To use wiki repository on the host, mount it, i.e.:
   $ docker run -d -p 80:80 \
       -v /home/me/wiki:/root/wiki \
       deild/gollum --http

To run with strict HTTPS creating new self-signed keys:
   $ docker run -d -p 80:80 -p 443:443 \
       deild/gollum --hsts mybox.example.com

To run with strict HTTPS using your own keys, mount them, i.e.:
   $ docker run -d -p 80:80 -p 443:443 \
       -v /etc/ssl:/etc/ssl \
       deild/gollum --hsts mybox.example.com

   (the cert's CN must match the FQDN)

To run as a rack application, place your config file in the repo,
mount it, and set RACK_APP environment variable to its name, i.e.:
   $ docker run -d -p 80:80 \
       -v /home/me/wiki:/root/wiki \
       -e RACK_APP=config.ru \
       deild/gollum --http

To run using a time zone other than UTC, set the TIMEZONE environment
variable to the desired time zone (TZ), i.e.:
   $ docker run -d -p 80:80 \
       -e TIMEZONE=America/Los_Angeles \
       deild/gollum --http

To bypass script, just enter desired command, i.e.:
   $ docker run -it deild/gollum bash

Key paths in the container:
   /root/wiki     - Wiki content (a git repository)
   /etc/ssl       - SSL keys and certificates
   /etc/ssl/private/ssl-cert-snakeoil.key  - Private SSL key
   /etc/ssl/certs/ssl-cert-snakeoil.pem    - Public SSL cert
```

## Building the Docker image

```bash
docker build -t deild/gollum:latest -t deild/gollum:4.1.4 -t deild/gollum:4.1 -t deild/gollum:4 .
```

## Notes

- This image uses GitHub Flavored Markdown ([GFM][3]).
- This image uses [Asciidoctor][5].
- For a rack application, see the example `config.ru`, and be sure to append its required packages and gems to the respective RUN commands in the `Dockerfile`.
- See the [TZ Database][4] for the available values for the `$TIMEZONE` environment variable.

[1]: https://github.com/gollum/gollum
[2]: https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security
[3]: https://guides.github.com/features/mastering-markdown/
[4]: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
[5]: https://asciidoctor.org/
[6]: https://github.com/genebarker/gollum
