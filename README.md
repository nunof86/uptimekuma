# Documentation

## See the full documentation on https://documentation-com.gitbook.io/uptime-kuma-installation/
# Prerequisites

## System Update

```bash
sudo apt update && sudo apt full-upgrade -y
```

## Install Required Packages

```bash
sudo apt install apt-transport-https ca-certificates software-properties-common -y
```

## Docker Installation - Ubuntu

### **Add Docker APT Key**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```

### **Add Docker APT Repository**

```bash
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```

## Docker Installation - Debian

### **Add Docker APT Key**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```

### **Add Docker APT Repository**

```bash
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```

## **Install Docker**

```bash
sudo apt-get update
sudo apt-get install docker-ce -y
```

## **Download and Install Docker Compose**

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

## Ansible Installation

```bash
sudo apt install ansible -y
```

# UpTime Kuma Docker Deployment

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
