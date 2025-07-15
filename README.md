# Ansible Pull Docker Compose Demo 

This repository contains a demo of how to use `ansible-pull` to monitor a git repository for changes and apply them to a running system.

## How it works

Ansible Pull is a feature in Ansible that inverts the standard "push" model of Ansible, where the control machine pushes configurations to managed nodes. In Ansible Pull, managed nodes (clients) "pull" playbooks from a central source (like a Git repository) and execute them locally

This demo uses `ansible-pull` to run a docker-compose file that starts a simple load-balancing demo application.

## Setup (Ubuntu)

1. **Install Ansible:**

```bash
sudo apt-get update && sudo apt-get install ansible -y
```

2. **Run ansible-pull for the first time:**

```bash
/usr/bin/ansible-pull -U https://github.com/JManzur/ansible-pull-docker-compose.git -d /opt/ansible-pull
```

3. **Check the status of the service:** The previous command will create a systemd service and timer to run `ansible-pull` on a schedule and check for changes in the repository.

```bash
systemctl status ansible-pull.timer
```

4. **List the listers:**

```bash
systemctl list-timers | grep ansible-pull.timer
```

### Testing the timer

1. Update the `docker-compose.yml` file in the repository to change the `APP_VERSION` environment variable to a new version (e.g., from `v1.0.0` to `v1.0.1`).
2. Push the changes to the Git repository.
3. Wait for the timer to trigger.
4. Check the `journalctl`logs

```bash
journalctl -u ansible-pull.service -f
```
You can also use `systemctl show` to check the last run time of the timer:

```bash
systemctl show ansible-pull.timer --property=LastTriggerUSec
```
Or the next scheduled run time:

```bash
systemctl show ansible-pull.timer --property=NextElapseUSec
```

5. Verify that the application has been updated by sending a request to the application:

```bash
curl http://localhost:8882/status | jq -r
```   