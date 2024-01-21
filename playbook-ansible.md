# Playbook - Ansible

## Ansible Playbook



1. Replace the <mark style="color:red;">`hosts:`</mark> option to <mark style="color:red;">**localhost**</mark> or other <mark style="color:red;">**host**</mark> of your choice.

```yaml
- name: UpTime Kuma Installation
  hosts: localhost
  become: true
  any_errors_fatal: true
  max_fail_percentage: 0

  tasks:
    - name: System Update
      apt:
        update_cache: yes
      changed_when: false

    - name: Dist Upgrade
      apt:
        upgrade: dist
      register: upgrade_output

    - name: Install Required Packages
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - software-properties-common
        state: present

    - name: Add Docker APT Key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker APT Repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_lsb.codename }} stable
        state: present

    - name: Update apt Cache
      apt:
        update_cache: yes

    - name: Install Docker
      apt:
        name: docker-ce
        state: latest

    - name: Download and Install Docker Compose
      get_url:
        url: https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64
        dest: /usr/bin/docker-compose
        mode: '0755'

    - name: Create the Kuma Repository
      file:
        path: /home/nuno/Documents/kuma
        state: directory

    - name: Create the docker-compose.yml Configuration File
      copy:
        content: |
          version: '3.3'

          services:
            uptime-kuma:
              image: louislam/uptime-kuma:1
              container_name: uptime-kuma
              volumes:
                - ./uptime-kuma-data:/app/data
              ports:
                - 3001:3001
        dest: /home/nuno/Documents/kuma/docker-compose.yml

    - name: Start UpTime Kuma Container
      command: docker-compose up -d
      args:
        chdir: /home/nuno/Documents/kuma
```
