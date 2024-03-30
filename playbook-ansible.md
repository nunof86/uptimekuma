# Playbook - Ansible

## Ansible Playbook



1. Replace the <mark style="color:red;">`hosts:`</mark> option to <mark style="color:red;">**localhost**</mark> or other <mark style="color:red;">**host**</mark> of your choice.

```yaml
---

- name: UpTime Kuma Installation
  hosts: localhost
  become: true
  any_errors_fatal: true
  max_fail_percentage: 0

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: System update
      apt:
        update_cache: yes
      changed_when: false
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Dist upgrade
      apt:
        upgrade: dist
      register: upgrade_output
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Install required packages
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - software-properties-common
          - curl
        state: present
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Add Docker APT key to trusted.gpg.d
      shell: "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg"
      args:
        warn: no
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Add Docker APT repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_lsb.codename }} stable
        state: present
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Install Docker
      apt:
        name: docker-ce
        state: latest
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Download and Install Docker Compose
      get_url:
        url: https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64
        dest: /usr/bin/docker-compose
        mode: '0755'
      when: ansible_distribution in ['Ubuntu', 'Debian']

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
