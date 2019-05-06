# Usage

This is meant to be used with a Pi on [Hypriot](http://hypriot.com). Installing it with their tool, [flash](https://github.com/hypriot/flash), is pretty simple.

It has been tested only on Hypriot 1.9.

## On host

Add your SSH key to the pi (On OSX? You may need `brew install ssh-copy-id`):

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pirate@ip.address
```

Here on, you can do the following remotely.

## Configure Pi

SSH into your pi.

```sh
sudo apt-get update && \
  sudo apt-get install python3.4-minimal python3.4 python-crypto python-markupsafe python-jinja2 python-paramiko python-pkg-resources python-setuptools python-pip python-yaml -y && \
  sudo pip install ansible passlib
```

## Ansible

Run the specific role you want. Remember:

- Copy `pi.sample` to `pi` and edit the ip addresses. If you have multiple Pis you want to configure now, you may add them as extra lines
- In the following commands, replace `ip.address` with the Pi's ip address
- Copy `group_vars/secrets.yml.sample` to `group_vars/secrets.yml` and define your own variables

## Playbooks

### bitorrent_client

This playbook will fetch different roles to set your server with the following roles:
  - common packages and configuration
  - secure the server
  - install deluge bitorrent client
  - install nginx to serve files in deluge download directory (secured by http basic)
  - setup cloudflare domain to be updated in case of ip change

To install everything:

```sh
ansible-playbook bittorrent_client.yml -i pi
```

Alternatively you can run ansible roles individually by specifying tags if you need:

```sh
ansible-playbook bittorrent_client.yml -i pi --tags "nginx"
```

Or skip some:

```sh
ansible-playbook bittorrent_client.yml -i pi --skip-tags "common,cloudflare"
```
