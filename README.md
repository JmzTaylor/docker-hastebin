# Hastebin

## Features

- Based on Alpine Linux.
- Latest code from [seejohnrun/haste-server](https://github.com/seejohnrun/haste-server)
- Ran as an unprivileged user (see `UID` and `GID`)
- Uses the default file storage driver (no expiration).

## Build-time variables

- **`HASTEBIN_VER`**: A commit or a branch since the repo doesn't have tags (default: `master`)

## Environment variables

- **`GID`**: user id *(default: `4242`)*
- **`UID`**: group id *(default: `4242`)*

## Usage

```sh
docker run -d \
  --name hastebin \
  -p 80:7777 \
  jmzsoftware/docker-hastebin:latest
```

As said above, the container will run as `4242:4242` by default, but you can specify the `UID` and `GID` yourself:

```sh
docker run -d \
  --name hastebin \
  -p 80:7777 \
  -e UID=4242 \
  -e GID=4242 \
  jmzsoftware/docker-hastebin:latest
```

### Volume

By default, the container will create a volume to store `/app/data`. This is where your pastes will be stored.

You can specify a volume yourself:

```sh
docker run -d \
  --name hastebin \
  --mount source=hastebin,target=/app/data \
  -p 80:7777 \
  -e UID=4242 \
  -e GID=4242 \
  jmzsoftware/docker-hastebin:latest
```

Or use a bind mount:

```sh
docker run -d \
  --name hastebin \
  --mount type=bind,source="$(pwd)"/data,target=/app/data \
  -p 80:7777 \
  -e UID=4242 \
  -e GID=4242 \
  jmzsoftware/docker-hastebin:latest
```

### Docker Compose

A `docker-compose.yml` example:

```yml
version: '2.3'

services:
  hastebin:
    container_name: hastebin
    image: jmzsoftware/docker-hastebin:latest
    restart: always
    volumes:
      - ./data:/app/data
    ports:
      - "80:7777"
    environment:
     - UID=4242
     - GID=4242
```
