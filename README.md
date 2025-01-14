  # MediaWiki Parsoid with Docker

[![Docker Pulls](https://img.shields.io/docker/pulls/scwiki/parsoid.svg?style=flat-square)](https://hub.docker.com/r/scwiki/parsoid/)

This repo contains a [Docker](https://docs.docker.com/) image to run the [Parsoid](https://www.mediawiki.org/wiki/Parsoid) application. See the full [Parsoid/Setup documentation](https://www.mediawiki.org/wiki/Parsoid/Setup#Docker) for help.

## Versions available

`scwiki/parsoid:0.12.0`

## What Is Included?
- Alpine
- Parsoid

## How to deploy
To start [Parsoid](https://www.mediawiki.org/wiki/Parsoid) run the command below. Just pay attention to the MediaWiki version and choose a compatible Parsoid version.

```
# For MediaWiki >= 1.35
docker run -d -p 8080:8000 -e PARSOID_ENABLE_LINTER=true -e PARSOID_DOMAIN_localhost=http://localhost/w/api.php scwiki/parsoid:0.12.0
```

## Examples

How to add more than one domain:

```
docker run -d -p 8080:8000 \
            -e PARSOID_DOMAIN_foobar=http://foobar.com/w/api.php \
            -e PARSOID_DOMAIN_example=http://example.com/w/api.php \
            -e PARSOID_DOMAIN_localhost=http://localhost/w/api.php \
            scwiki/parsoid:0.12.0
```

How to expose on a specific port: (You can use arbitrary port numbers which are not already in use)

```
# Expose port 8081
docker run -d -p 8081:8000 -e PARSOID_DOMAIN_localhost=http://localhost/w/api.php scwiki/parsoid:0.12.0

# Expose port 8142
docker run -d -p 8142:8000 -e PARSOID_DOMAIN_localhost=http://localhost/w/api.php scwiki/parsoid:0.12.0
```

## Settings (ENV vars)

- `PARSOID_DOMAIN_{domain}` defines URI and domain for the Parsoid service. The value of `{domain}` should be the same as the `MW_REST_DOMAIN` parameter in the MediaWiki web container. You can specify such variables multiple times (one for each domain the service should run for)
- `PARSOID_NUM_WORKERS` defines the number of worker processes to the parsoid service. Set to `0` to run everything in a single process without clustering. Use `ncpu` to run as many workers as there are CPU units.
- `PARSOID_LOGGING_LEVEL` by default `info`
- `PARSOID_STRICT_SSL` by default `true` [`true`, `false`]
- `PARSOID_ENABLE_LINTER` by default `false` [`true`, `false`] - Needed for Extension:DiscussionTools

For example, the environment variable `PARSOID_DOMAIN_web=http://web/w/api.php` creates following section in the Parsoid configuration:
```
mwApis:
  -
    uri: 'http://web/w/api.php'
    domain: 'web'
```

## Thanks

- [pastakhov](https://github.com/pastakhov): Creator of the original code base.
- [thenets](https://github.com/thenets): Maintainer of the second fork.
- [muellermartin](https://github.com/muellermartin): Improved the documentation.
- [endoodev1](https://github.com/endoodev1): Helped with Kubernetes compatibility.
- [sepastian](https://github.com/sepastian): Added support to `strictSSL` using container ENV.
