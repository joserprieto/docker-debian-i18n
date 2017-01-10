# What is Debian?

Debian is an operating system which is composed primarily of free and open-source software, most of which is under the 
GNU General Public License, and developed by a group of individuals known as the Debian project. Debian is one of the
most popular Linux distributions for personal computers and network servers, and has been used as a base for several
other Linux distributions.

> [wikipedia.org/wiki/Debian](https://en.wikipedia.org/wiki/Debian)


# About this image

This image is based on Debian Official Image:

> [Debian Official Image](https://hub.docker.com/_/debian/)
> [Debian Official Image Repository](https://github.com/tianon/docker-brew-debian)

But add extra support for configure timezone and locales before built the image, with the arguments in the:

	docker build

command (--build-arg), and

	ARG

in the Dockerfile.

Default values for the timezone and locale are:

	Timezone:	"Europe/Madrid"
	Locale:		es_ES.UTF-8


## Locales

The locales are generated with the snippet described in Debian Official Image, and [used in PostgreSQL Official Image]
(https://github.com/docker-library/postgres/blob/69bc540ecfffecce72d49fa7e4a46680350037f9/9.6/Dockerfile#L21-L24):

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


## `<suite>-slim` variants

Not yet...

## How It's Made

For see how it's made the official debian image, see the original repo.

> [Debian Official Image Repository](https://github.com/tianon/docker-brew-debian)

This image only adds support for timezonen and locale, as we explain before.
