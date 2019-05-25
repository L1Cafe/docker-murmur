## Description

This is a barebones docker container built using busybox and a statically compiled version of murmur from the [official website](https://www.mumble.info).

It's configured to look for the configuration file in `/etc/murmur.ini`.

## Usage

The recommended way to run this container is as follows:

```bash
$ docker run --name murmur -d -p 64738:64738 -p 64738:64738/udp \
    -v /path/to/data:/data hellwyvern/murmur
```

Replace `/path/to/data` to the path you want Mumble to save its data in your Docker host.

## Important notes

### Getting the super-user password

On first run, if you don't already have an existing state database, you'll want to look at the logs for your container to get the super-user password:

```bash
$ docker logs murmur 2>&1 | grep Password
<W>2014-07-27 01:41:31.256 1 => Password for 'SuperUser' set to '(mAq3hkwnkD'
```

### Providing your own murmur.ini

If you want to tweak the provided murmur.ini, you should run:

```bash
docker cp murmur:/etc/murmur.ini /path/to/murmur.ini
```

If you are using the built in volume at `/data/`, then you should ensure your config contains:

```ini
database=/data/murmur.sqlite
```

To run the container with your tweaked murmur.ini:

```bash
$ docker run -d -p 64738:64738 -p 64738:64738/udp \
    -v /path/to/murmur.ini:/etc/murmur.ini hellwyvern/murmur
```