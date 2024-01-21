# UpTime Kuma Installation

## Creating a Directory

1. First we will need to create a directory to the UpTime Kuma with the command bellow.

```bash
mkdir /home/nuno/Documents/kuma
```

## Create a Docker-Compose File

1. Second we will create a docker-compose file.

```bash
nano /home/nuno/Documents/kuma/docker-compose.yml
```

2. Next paste the following configuration:

```yaml
version: '3.3'

services:
  uptime-kuma:
    image: louislam/uptime-kuma:1
    container_name: uptime-kuma
    volumes:
      - ./uptime-kuma-data:/app/data
    ports:
      - 3001:3001
    restart: always
```

## Docker Compose

1. Start the UpTime Kuma deployment using docker-compose:

```bash
docker-compose up -d
```

2. After that navigate to the dashboard with <mark style="color:red;">`http://your_ip_address`</mark>.
