# ansible-jitsi

Installs jitsi via several docker container.

Tested on:
* Ubuntu 16.04

## System requirements

* Docker
* docker-compose
* Systemd

## Role requirements

* python-docker package

## Tasks

* Build docker image locally
* Create volume paths for docker container
* Create docker-compose file
* Setup systemd unit file
* Start/Restart service

## Role parameters

| Variable      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| volume  | text   | no       | <empty>  | Local path to jitsi config and data |
| publish.web     | text | no | <empty> | Web Port to be published                     |
| publish.https     | text | no | <empty> | Https Port to be published (but not used)                    |
| publish.interface | text | no | 0.0.0.0 | Interface to be published               |
| secrets | object | yes | <empty> | Secret keys and passwords used by the containers |

## Usage

### requirements.yml

```yaml
- name: install-jitsi
  src: https://github.com/borisskert/ansible-meet-jitsi.git
  scm: git
```

### Example Playbook

Usage

```yaml
    - role: install-jitsi
      volume: /srv/docker/jitsi
      publish:
        web: 10080
        https: 10443
        interface: "{{docker_network_interface}}"
        url: 'https://meet.my.jitsi'
      secrets: # you can generate them randomly
        JICOFO_COMPONENT_SECRET: 'my secret 1'
        JICOFO_AUTH_PASSWORD: 'my secret 2'
        JVB_AUTH_PASSWORD: 'my secret 3'
        JIGASI_XMPP_PASSWORD: 'my secret 4'
        JIBRI_RECORDER_PASSWORD: 'my secret 5'
        JIBRI_XMPP_PASSWORD: 'my secret 6'
```

# Test this role

Requirements:

* ansible
* vagrant
* VirtualBox

```shell script
$ cd tests
$ ./test.sh
```
