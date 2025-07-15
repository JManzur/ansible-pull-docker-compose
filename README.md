# Ansible Pull Docker Compose Demo 

This repository contains a demo of how to use `ansible-pull` to monitor a git repository for changes and apply them to a running system.

## How it works

Ansible Pull is a feature in Ansible that inverts the standard "push" model of Ansible, where the control machine pushes configurations to managed nodes. In Ansible Pull, managed nodes (clients) "pull" playbooks from a central source (like a Git repository) and execute them locally

This demo uses `ansible-pull` to run a docker-compose file that starts a simple load-balancing demo application.

## Setup

### Ubuntu

1. **Install Ansible:**

```bash
sudo apt-get update && sudo apt-get install ansible -y
```

2. **Run ansible-pull:**

```bash
/usr/bin/ansible-pull -U https://github.com/JManzur/ansible-pull-docker-compose.git -d /opt/ansible-pull
```a