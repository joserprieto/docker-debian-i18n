# Supported tags and respective `Dockerfile` links

-   [`8.6-es-es`, `8-es-es`, `jessie-es-es`, `latest-es-es`, `latest`, (*Dockerfile*)](https://github.com/joserprieto/docker-debian-i18n/blob/master/Dockerfile)
-   [`8.6-fr-fr`, `8-fr-fr`, `jessie-fr-fr`, `latest-fr-fr`, (*Dockerfile*)](https://github.com/joserprieto/docker-debian-i18n/blob/master/Dockerfile)

The Dockerfile allows the localization of locales and timezone, so, we have more than one tag with the same Dockerfile

From the [Debian Official Image](https://hub.docker.com/_/debian/):

For more information about this image and its history, please see [the relevant manifest file (`library/alpine`)](https://github.com/docker-library/official-images/blob/master/library/alpine). This image is updated via [pull requests to the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images/pulls?q=label%3Alibrary%2Falpine).

For detailed information about the virtual/transfer sizes and individual layers of each of the above supported tags, please see [the `repos/alpine/tag-details.md` file](https://github.com/docker-library/repo-info/blob/master/repos/alpine/tag-details.md) in [the `docker-library/repo-info` GitHub repo](https://github.com/docker-library/repo-info).



# What is Debian?

Debian is an operating system which is composed primarily of free and open-source software, most of which is under the 
GNU General Public License, and developed by a group of individuals known as the Debian project. Debian is one of the
most popular Linux distributions for personal computers and network servers, and has been used as a base for several
other Linux distributions.

> [wikipedia.org/wiki/Debian](https://en.wikipedia.org/wiki/Debian)

![logo](./logo.png)

# About this image

This image is based on Debian Official Image:

> [Debian Official Image](https://hub.docker.com/_/debian/)
> [Debian Official Image Repository](https://github.com/tianon/docker-brew-debian)

But add extra support for configure timezone and locales before built the image, with the arguments in 
the `docker build` command (`--build-arg`), and `ARG` in the `Dockerfile`.

Default values for the timezone and locale are:

```bash
Timezone:	"Europe/Madrid"
Locale:		es_ES.UTF-8
```

# How to use this image.

## Build a localized image.

To build a localized image, this is, a Debian Image with locale and timezone correctly defined, we have to use the 
`--build-arg` parameter of the `docker build` command.

An example:

```bash
docker build --build-arg TIMEZONE="Europe/France" --build-arg LOCALE_LANG_COUNTRY="fr_FR" .
```

## Locales

The locales are generated with the snippet described in Debian Official Image, and 
[used in PostgreSQL Official Image](https://github.com/docker-library/postgres/blob/69bc540ecfffecce72d49fa7e4a46680350037f9/9.6/Dockerfile#L21-L24):

```dockerfile
RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8
```

We only add a few arguments for the build:

```dockerfile
ARG LOCALE_LANG_COUNTRY="es_ES"
ARG LOCALE_CODIFICATION="UTF-8"
ARG LOCALE_CODIFICATION_ENV="utf8"
```

And execute the build as:


```dockerfile
    echo "=> Configuring and installing locale (${LOCALE_LANG_COUNTRY}.${LOCALE_CODIFICATION}):" && \
        DEBIAN_FRONTEND=noninteractive apt-get install -y locales && \
        rm -rf /var/lib/apt/lists/* && \
        localedef -i ${LOCALE_LANG_COUNTRY} -c -f ${LOCALE_CODIFICATION} -A /usr/share/locale/locale.alias ${LOCALE_LANG_COUNTRY}.${LOCALE_CODIFICATION}
```

## Timezone

We use a snippet provided by [Oscar](https://oscarmlage.com/) (Thanks!):

```dockerfile
    echo "=> Configuring and installing timezone:" && \
        echo "Europe/Madrid" > /etc/timezone && \
        dpkg-reconfigure -f noninteractive tzdata && \
```

Only added an ARG for the build:

```dockerfile
ARG TIMEZONE="Europe/Madrid"
```

And, finally:

```dockerfile
    echo "=> Configuring and installing timezone (${TIMEZONE}):" && \
        echo ${TIMEZONE} > /etc/timezone && \
        dpkg-reconfigure -f noninteractive tzdata && \
```

Obviously, the value of the timezone has to be one of the right values:

[Change Timezone on Debian](https://wiki.debian.org/TimeZoneChanges)
[List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)


# `<suite>-slim` variants

Not yet...

# How It's Made

For see how it's made the official debian image, see the original repo.

> [Debian Official Image Repository](https://github.com/tianon/docker-brew-debian)

This image only adds support for timezone and locale, as we explain before.

# Supported Docker versions

This image is officially supported on Docker version 1.12.5 -from the [Debian Official Image](https://hub.docker.com/_/debian/)- .

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see [the Docker installation documentation](https://docs.docker.com/installation/) for details on how to upgrade your Docker daemon.

# User Feedback

## Issues

If you have any problems with or questions about this image, please contact us through a 
[GitHub issue](https://github.com/joserprieto/docker-debian-i18n/issues).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive 
pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a 
[GitHub issue](https://github.com/joserprieto/docker-debian-i18n/issues), especially for more ambitious contributions. 
This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help 
you find out if someone else is working on the same thing.

## Documentation

To define how and where will be stored....